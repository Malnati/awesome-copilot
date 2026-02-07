---
name: copilot-sdk
description: Crie aplicacoes agenticas com GitHub Copilot SDK. Use ao embutir agentes de IA em apps, criar tools customizadas, implementar respostas em streaming, gerenciar sessoes, conectar a servidores MCP ou criar agentes customizados. Dispara em Copilot SDK, GitHub SDK, agentic app, embed Copilot, programmable agent, MCP server, custom agent.
---

# GitHub Copilot SDK

Embuta workflows agenticos do Copilot em qualquer aplicacao usando Python, TypeScript, Go ou .NET.

## Visao Geral

O GitHub Copilot SDK expoe o mesmo engine por tras do Copilot CLI: um runtime de agentes testado em producao que voce pode invocar programaticamente. Nao ha necessidade de criar sua propria orquestracao — voce define o comportamento do agente, o Copilot lida com planejamento, invocacao de tools, edicoes de arquivos e mais.

## Pre-requisitos

1. **GitHub Copilot CLI** instalado e autenticado ([Guia de instalacao](https://docs.github.com/en/copilot/how-tos/set-up/install-copilot-cli))
2. **Runtime de linguagem**: Node.js 18+, Python 3.8+, Go 1.21+ ou .NET 8.0+

Verifique o CLI: `copilot --version`

## Instalacao

### Node.js/TypeScript
```bash
mkdir copilot-demo && cd copilot-demo
npm init -y --init-type module
npm install @github/copilot-sdk tsx
```

### Python
```bash
pip install github-copilot-sdk
```

### Go
```bash
mkdir copilot-demo && cd copilot-demo
go mod init copilot-demo
go get github.com/github/copilot-sdk/go
```

### .NET
```bash
dotnet new console -n CopilotDemo && cd CopilotDemo
dotnet add package GitHub.Copilot.SDK
```

## Inicio Rapido

### TypeScript
```typescript
import { CopilotClient } from "@github/copilot-sdk";

const client = new CopilotClient();
const session = await client.createSession({ model: "gpt-4.1" });

const response = await session.sendAndWait({ prompt: "What is 2 + 2?" });
console.log(response?.data.content);

await client.stop();
process.exit(0);
```

Run: `npx tsx index.ts`

### Python
```python
import asyncio
from copilot import CopilotClient

async def main():
    client = CopilotClient()
    await client.start()

    session = await client.create_session({"model": "gpt-4.1"})
    response = await session.send_and_wait({"prompt": "What is 2 + 2?"})

    print(response.data.content)
    await client.stop()

asyncio.run(main())
```

### Go
```go
package main

import (
    "fmt"
    "log"
    "os"
    copilot "github.com/github/copilot-sdk/go"
)

func main() {
    client := copilot.NewClient(nil)
    if err := client.Start(); err != nil {
        log.Fatal(err)
    }
    defer client.Stop()

    session, err := client.CreateSession(&copilot.SessionConfig{Model: "gpt-4.1"})
    if err != nil {
        log.Fatal(err)
    }

    response, err := session.SendAndWait(copilot.MessageOptions{Prompt: "What is 2 + 2?"}, 0)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println(*response.Data.Content)
    os.Exit(0)
}
```

### .NET (C#)
```csharp
using GitHub.Copilot.SDK;

var client = new CopilotClient();
await client.StartAsync();

var session = await client.CreateSessionAsync(new SessionConfig { Model = "gpt-4.1" });
var response = await session.SendAndWaitAsync(new MessageOptions { Prompt = "What is 2 + 2?" });

Console.WriteLine(response?.Data?.Content);

await client.StopAsync();
```

## Criando Tools

Crie tools customizadas para adicionar capacidades ao seu agente:

```typescript
import { ToolRegistry } from "@github/copilot-sdk";

const tools = new ToolRegistry();

// Register a tool
await tools.register({
    name: "getWeather",
    description: "Get weather by city name",
    inputSchema: {
        type: "object",
        properties: {
            city: { type: "string" }
        },
        required: ["city"]
    },
    handler: async ({ city }) => {
        return { temp: 72, conditions: "sunny" };
    }
});
```

## Streaming de Respostas

```typescript
const responseStream = await session.send({ prompt: "Write a short story" });

for await (const message of responseStream) {
    if (message.type === "content") {
        process.stdout.write(message.data);
    }
}
```

## Workflows Avancados

### Multi-turn Conversations

```typescript
const session = await client.createSession({ model: "gpt-4.1" });

await session.sendAndWait({ prompt: "Summarize this repo." });
await session.sendAndWait({ prompt: "Now extract action items." });
```

### Agente com Tools

```typescript
const tools = new ToolRegistry();
await tools.register({
    name: "runShell",
    description: "Run a shell command",
    inputSchema: {
        type: "object",
        properties: { cmd: { type: "string" } },
        required: ["cmd"]
    },
    handler: async ({ cmd }) => {
        // Execute command here
        return { output: "ok" };
    }
});

const client = new CopilotClient({ tools });
```

## Notas de Seguranca

- Nunca passe secrets diretamente nos prompts
- Use variaveis de ambiente para credenciais
- Limite tokens ao menor privilegio necessario
- Audite handlers de tools para risco de command injection

## Problemas Comuns

| Issue | Fix |
|------|-----|
| `copilot: command not found` | Instale o GitHub Copilot CLI e adicione ao PATH |
| `Unauthorized` | Execute `copilot auth login` |
| `Session timeout` | Recrie a sessao ou atualize o auth |
