---
name: mcp-cli
description: Interface para servidores MCP (Model Context Protocol) via CLI. Use quando precisar interagir com tools externas, APIs ou fontes de dados via servidores MCP, listar servidores/tools MCP disponiveis ou chamar tools MCP pela linha de comando.
---

# MCP-CLI

Acesse servidores MCP pela linha de comando. MCP habilita interacao com sistemas externos como GitHub, filesystems, databases e APIs.

## Comandos

| Comando                            | Saida                          |
| ---------------------------------- | ------------------------------ |
| `mcp-cli`                          | Listar todos os servidores e tools |
| `mcp-cli <server>`                 | Mostrar tools com parametros   |
| `mcp-cli <server>/<tool>`          | Obter schema JSON da tool      |
| `mcp-cli <server>/<tool> '<json>'` | Chamar a tool com argumentos   |
| `mcp-cli grep "<glob>"`            | Buscar tools pelo nome         |

**Adicione `-d` para incluir descricoes** (ex.: `mcp-cli filesystem -d`)

## Workflow

1. **Descobrir**: `mcp-cli` → veja servidores e tools disponiveis
2. **Explorar**: `mcp-cli <server>` → veja tools com parametros
3. **Inspecionar**: `mcp-cli <server>/<tool>` → obtenha schema JSON completo de entrada
4. **Executar**: `mcp-cli <server>/<tool> '<json>'` → execute com argumentos

## Exemplos

```bash
# List all servers and tool names
mcp-cli

# See all tools with parameters
mcp-cli filesystem

# With descriptions (more verbose)
mcp-cli filesystem -d

# Get JSON schema for specific tool
mcp-cli filesystem/read_file

# Call the tool
mcp-cli filesystem/read_file '{"path": "./README.md"}'

# Search for tools
mcp-cli grep "*file*"

# JSON output for parsing
mcp-cli filesystem/read_file '{"path": "./README.md"}' --json

# Complex JSON with quotes (use heredoc or stdin)
mcp-cli server/tool <<EOF
{"content": "Text with 'quotes' inside"}
EOF

# Or pipe from a file/command
cat args.json | mcp-cli server/tool

# Find all TypeScript files and read the first one
mcp-cli filesystem/search_files '{"path": "src/", "pattern": "*.ts"}' --json | jq -r '.content[0].text' | head -1 | xargs -I {} sh -c 'mcp-cli filesystem/read_file "{\"path\": \"{}\"}"'
```

## Opcoes

| Flag         | Finalidade              |
| ------------ | ----------------------- |
| `-j, --json` | JSON output para scripts |
| `-r, --raw`  | Raw text content         |
| `-d`         | Incluir descricoes       |

## Exit Codes

- `0`: Sucesso
- `1`: Erro de cliente (bad args, missing config)
- `2`: Erro de servidor (tool failed)
- `3`: Erro de rede
