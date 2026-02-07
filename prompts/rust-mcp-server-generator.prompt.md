---
name: rust-mcp-server-generator
description: 'Gere um projeto completo de servidor Model Context Protocol em Rust com tools, prompts, resources e testes usando o SDK oficial rmcp'
agent: agent
---

# Gerador de Servidor MCP em Rust

Voce e um gerador de servidor MCP em Rust. Crie um projeto completo e pronto para producao usando o SDK oficial `rmcp`.

## Requisitos do Projeto

Pergunte ao usuario:
1. **Nome do projeto** (por exemplo, "my-mcp-server")
2. **Descricao do servidor** (por exemplo, "A weather data MCP server")
3. **Tipo de transport** (stdio, sse, http, ou todos)
4. **Tools a incluir** (por exemplo, "weather lookup", "forecast", "alerts")
5. **Se deve incluir prompts e resources**

## Estrutura do Projeto

Gere esta estrutura:

```
{project-name}/
├── Cargo.toml
├── .gitignore
├── README.md
├── src/
│   ├── main.rs
│   ├── handler.rs
│   ├── tools/
│   │   ├── mod.rs
│   │   └── {tool_name}.rs
│   ├── prompts/
│   │   ├── mod.rs
│   │   └── {prompt_name}.rs
│   ├── resources/
│   │   ├── mod.rs
│   │   └── {resource_name}.rs
│   └── state.rs
└── tests/
    └── integration_test.rs
```

## Templates de Arquivo

### Cargo.toml

```toml
[package]
name = "{project-name}"
version = "0.1.0"
edition = "2021"

[dependencies]
rmcp = { version = "0.8.1", features = ["server"] }
rmcp-macros = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
anyhow = "1.0"
tracing = "0.1"
tracing-subscriber = "0.3"
schemars = { version = "0.8", features = ["derive"] }
async-trait = "0.1"

# Optional: for HTTP transports
axum = { version = "0.7", optional = true }
tower-http = { version = "0.5", features = ["cors"], optional = true }

[dev-dependencies]
tokio-test = "0.4"

[features]
default = []
http = ["dep:axum", "dep:tower-http"]

[[bin]]
name = "{project-name}"
path = "src/main.rs"
```

### .gitignore

```gitignore
/target
Cargo.lock
*.swp
*.swo
*~
.DS_Store
```

### README.md

```markdown
# {Project Name}

{Server description}

## Installation

```bash
cargo build --release
```

## Usage

### Stdio Transport

```bash
cargo run
```

### SSE Transport

```bash
cargo run --features http -- --transport sse
```

### HTTP Transport

```bash
cargo run --features http -- --transport http
```

## Configuration

Configure in your MCP client (e.g., Claude Desktop):

```json
{
  "mcpServers": {
    "{project-name}": {
      "command": "path/to/target/release/{project-name}",
      "args": []
    }
  }
}
```

## Tools

- **{tool_name}**: {Tool description}

## Development

Run tests:

```bash
cargo test
```

Run with logging:

```bash
RUST_LOG=debug cargo run
```
```

### src/main.rs

```rust
use anyhow::Result;
use rmcp::{
    protocol::ServerCapabilities,
    server::Server,
    transport::StdioTransport,
};
use tokio::signal;
use tracing_subscriber;

mod handler;
mod state;
mod tools;
mod prompts;
mod resources;

use handler::McpHandler;

#[tokio::main]
async fn main() -> Result<()> {
    // Initialize tracing
    tracing_subscriber::fmt()
        .with_max_level(tracing::Level::INFO)
        .with_target(false)
        .init();
    
    tracing::info!("Starting {project-name} MCP server");
    
    // Create handler
    let handler = McpHandler::new();
    
    // Create transport (stdio by default)
    let transport = StdioTransport::new();
    
    // Build server with capabilities
    let server = Server::builder()
        .with_handler(handler)
        .with_capabilities(ServerCapabilities {
            tools: Some(Default::default()),
            prompts: Some(Default::default()),
            resources: Some(Default::default()),
            ..Default::default()
        })
        .build(transport)?;
    
    tracing::info!("Server started, waiting for requests");
    
    // Run server until Ctrl+C
    server.run(signal::ctrl_c()).await?;
    
    tracing::info!("Server shutting down");
    Ok(())
}
```

### src/handler.rs

```rust
use rmcp::{
    model::*,
    protocol::*,
    server::{RequestContext, ServerHandler, RoleServer, ToolRouter},
    ErrorData,
};
use rmcp::{tool_router, tool_handler};
use async_trait::async_trait;

use crate::state::ServerState;
use crate::tools;

pub struct McpHandler {
    state: ServerState,
    tool_router: ToolRouter,
}

#[tool_router]
impl McpHandler {
    // Include tool definitions from tools module
    #[tool(
        name = "example_tool",
        description = "An example tool",
        annotations(read_only_hint = true)
    )]
    async fn example_tool(params: Parameters<tools::ExampleParams>) -> Result<String, String> {
        tools::example::execute(params).await
    }
    
    pub fn new() -> Self {
        Self {
            state: ServerState::new(),
            tool_router: Self::tool_router(),
        }
    }
}

#[tool_handler]
#[async_trait]
impl ServerHandler for McpHandler {
    async fn list_prompts(
        &self,
        _request: Option<PaginatedRequestParam>,
        _context: RequestContext<RoleServer>,
    ) -> Result<ListPromptsResult, ErrorData> {
        let prompts = vec![
            Prompt {
                name: "example-prompt".to_string(),
                description: Some("An example prompt".to_string()),
                arguments: Some(vec![
                    PromptArgument {
                        name: "topic".to_string(),
                        description: Some("The topic to discuss".to_string()),
                        required: Some(true),
                    },
                ]),
            },
        ];
        
        Ok(ListPromptsResult { prompts })
    }
    
    async fn get_prompt(
        &self,
        request: GetPromptRequestParam,
        _context: RequestContext<RoleServer>,
    ) -> Result<GetPromptResult, ErrorData> {
        match request.name.as_str() {
            "example-prompt" => {
                let topic = request.arguments
                    .as_ref()
                    .and_then(|args| args.get("topic"))
                    .ok_or_else(|| ErrorData::invalid_params("topic required"))?;
                
                Ok(GetPromptResult {
                    description: Some("Example prompt".to_string()),
                    messages: vec![
                        PromptMessage::user(format!("Let's discuss: {}", topic)),
                    ],
                })
            }
            _ => Err(ErrorData::invalid_params("Unknown prompt")),
        }
    }
    
    async fn list_resources(
        &self,
        _request: Option<PaginatedRequestParam>,
        _context: RequestContext<RoleServer>,
    ) -> Result<ListResourcesResult, ErrorData> {
        let resources = vec![
            Resource {
                uri: "example://data/info".to_string(),
                name: "Example Resource".to_string(),
                description: Some("An example resource".to_string()),
                mime_type: Some("text/plain".to_string()),
            },
        ];
        
        Ok(ListResourcesResult { resources })
    }
    
    async fn read_resource(
        &self,
        request: ReadResourceRequestParam,
        _context: RequestContext<RoleServer>,
    ) -> Result<ReadResourceResult, ErrorData> {
        match request.uri.as_str() {
            "example://data/info" => {
                Ok(ReadResourceResult {
                    contents: vec![
                        ResourceContents::text("Example resource content".to_string())
                            .with_uri(request.uri)
                            .with_mime_type("text/plain"),
                    ],
                })
            }
            _ => Err(ErrorData::invalid_params("Unknown resource")),
        }
    }
}
```

### src/state.rs

```rust
use std::sync::Arc;
use tokio::sync::RwLock;

#[derive(Clone)]
pub struct ServerState {
    // Add shared state here
    counter: Arc<RwLock<i32>>,
}

impl ServerState {
    pub fn new() -> Self {
        Self {
            counter: Arc::new(RwLock::new(0)),
        }
    }
    
    pub async fn increment(&self) -> i32 {
        let mut counter = self.counter.write().await;
        *counter += 1;
        *counter
    }
    
    pub async fn get(&self) -> i32 {
        *self.counter.read().await
    }
}
```

### src/tools/mod.rs

```rust
pub mod example;

pub use example::ExampleParams;
```

### src/tools/example.rs

```rust
use rmcp::model::Parameters;
use serde::{Deserialize, Serialize};
use schemars::JsonSchema;

#[derive(Debug, Deserialize, JsonSchema)]
pub struct ExampleParams {
    pub input: String,
}

pub async fn execute(params: Parameters<ExampleParams>) -> Result<String, String> {
    let input = &params.inner().input;
    
    // Tool logic here
    Ok(format!("Processed: {}", input))
}

#[cfg(test)]
mod tests {
    use super::*;
    
    #[tokio::test]
    async fn test_example_tool() {
        let params = Parameters::new(ExampleParams {
            input: "test".to_string(),
        });
        
        let result = execute(params).await.unwrap();
        assert!(result.contains("test"));
    }
}
```

### src/prompts/mod.rs

```rust
// Prompt implementations can go here if needed
```

### src/resources/mod.rs

```rust
// Resource implementations can go here if needed
```

### tests/integration_test.rs

```rust
use rmcp::{
    model::*,
    protocol::*,
    server::{RequestContext, ServerHandler, RoleServer},
};

// Replace with your actual project name in snake_case
// Example: if project is "my-mcp-server", use my_mcp_server
use my_mcp_server::handler::McpHandler;

#[tokio::test]
async fn test_list_tools() {
    let handler = McpHandler::new();
    let context = RequestContext::default();
    
    let result = handler.list_tools(None, context).await.unwrap();
    
    assert!(!result.tools.is_empty());
    assert!(result.tools.iter().any(|t| t.name == "example_tool"));
}

#[tokio::test]
async fn test_call_tool() {
    let handler = McpHandler::new();
    let context = RequestContext::default();
    
    let request = CallToolRequestParam {
        name: "example_tool".to_string(),
        arguments: Some(serde_json::json!({
            "input": "test"
        })),
    };
    
    let result = handler.call_tool(request, context).await;
    assert!(result.is_ok());
}

#[tokio::test]
async fn test_list_prompts() {
    let handler = McpHandler::new();
    let context = RequestContext::default();
    
    let result = handler.list_prompts(None, context).await.unwrap();
    assert!(!result.prompts.is_empty());
}

#[tokio::test]
async fn test_list_resources() {
    let handler = McpHandler::new();
    let context = RequestContext::default();
    
    let result = handler.list_resources(None, context).await.unwrap();
    assert!(!result.resources.is_empty());
}
```

## Diretrizes de Implementacao

1. **Use rmcp-macros**: Use macros `#[tool]`, `#[tool_router]` e `#[tool_handler]` para codigo limpo
2. **Type Safety**: Use `schemars::JsonSchema` para todos os tipos de parametro
3. **Error Handling**: Retorne `Result` com mensagens de erro adequadas
4. **Async/Await**: Todos os handlers devem ser async
5. **State Management**: Use `Arc<RwLock<T>>` para estado compartilhado
6. **Testing**: Inclua testes unitarios para tools e testes de integracao para handlers
7. **Logging**: Use macros `tracing` (`info!`, `debug!`, `warn!`, `error!`)
8. **Documentation**: Adicione doc comments em todos os itens publicos

## Padroes de Tool

### Simple Read-Only Tool

```rust
#[derive(Debug, Deserialize, JsonSchema)]
pub struct GreetParams {
    pub name: String,
}

#[tool(
    name = "greet",
    description = "Greets a user by name",
    annotations(read_only_hint = true, idempotent_hint = true)
)]
async fn greet(params: Parameters<GreetParams>) -> String {
    format!("Hello, {}!", params.inner().name)
}
```

### Tool com Error Handling

```rust
#[derive(Debug, Deserialize, JsonSchema)]
pub struct DivideParams {
    pub a: f64,
    pub b: f64,
}

#[tool(name = "divide", description = "Divides two numbers")]
async fn divide(params: Parameters<DivideParams>) -> Result<f64, String> {
    let p = params.inner();
    if p.b == 0.0 {
        Err("Cannot divide by zero".to_string())
    } else {
        Ok(p.a / p.b)
    }
}
```

### Tool com State

```rust
#[tool(
    name = "increment",
    description = "Increments the counter",
    annotations(destructive_hint = true)
)]
async fn increment(state: &ServerState) -> i32 {
    state.increment().await
}
```

## Rodando o Servidor Gerado

Depois de gerar:

```bash
cd {project-name}
cargo build
cargo test
cargo run
```

Para integracao com Claude Desktop:

```json
{
  "mcpServers": {
    "{project-name}": {
      "command": "path/to/{project-name}/target/release/{project-name}",
      "args": []
    }
  }
}
```

Agora gere o projeto completo com base nos requisitos do usuario!
