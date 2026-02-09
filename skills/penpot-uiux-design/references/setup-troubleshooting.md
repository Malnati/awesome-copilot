# Setup e Solucao de Problemas do Penpot MCP Server

Guia completo para instalar, configurar e solucionar problemas do Penpot MCP Server.

## Visao Geral de Arquitetura

A integracao do Penpot MCP exige **tres componentes** trabalhando juntos:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   MCP Client    │────▶│   MCP Server    │◀───▶│  Penpot Plugin  │
│ (VS Code/Claude)│     │  (port 4401)    │     │ (in browser)    │
└─────────────────┘     └────────┬────────┘     └────────┬────────┘
                                 │                       │
                                 │    WebSocket          │
                                 │    (port 4402)        │
                                 └───────────────────────┘
```

1. **MCP Server** - Expoe tools para seu AI client (HTTP na porta 4401)
2. **Plugin Server** - Serve os arquivos do plugin Penpot (HTTP na porta 4400)
3. **Penpot MCP Plugin** - Roda dentro do browser do Penpot e executa comandos de design

## Pre-requisitos

- **Node.js v22+** - [Download](https://nodejs.org/)
- **Git** - Para clonar o repositorio
- **Browser moderno** - Chrome, Firefox ou baseado em Chromium

Verifique a instalacao do Node.js:
```bash
node --version  # Should be v22.x or higher
npm --version
npx --version
```

## Instalacao

### Passo 1: Clonar e Instalar

```bash
# Clone the repository
git clone https://github.com/penpot/penpot-mcp.git
cd penpot-mcp

# Install dependencies
npm install
```

### Passo 2: Build e Iniciar Servers

```bash
# Build all components and start servers
npm run bootstrap
```

Este comando:

- Instala dependencias de todos os componentes
- Faz build do MCP server e do plugin
- Inicia ambos os servers (MCP em 4401, Plugin em 4400)

**Saida esperada:**

```txt
MCP Server listening on http://localhost:4401
Plugin server listening on http://localhost:4400
WebSocket server listening on port 4402
```

### Passo 3: Carregar o Plugin no Penpot

1. Abra o [Penpot](https://design.penpot.app/) in your browser
2. Abra ou crie um arquivo de design
3. Va ao menu **Plugins** (ou clique no icone de plugins)
4. Clique em **Load plugin from URL**
5. Enter: `http://localhost:4400/manifest.json`
6. A UI do plugin vai aparecer - clique em **"Connect to MCP server"**
7. O status deve mudar para **"Connected to MCP server"**

> **Importante**: Mantenha a UI do plugin aberta ao usar as ferramentas MCP. Ao fechar, o server desconecta.

### Passo 4: Configure seu MCP Client

#### VS Code com GitHub Copilot

Adicione no seu `settings.json` do VS Code:

```json
{
  "mcp": {
    "servers": {
      "penpot": {
        "url": "http://localhost:4401/sse"
      }
    }
  }
}
```

Ou use o endpoint HTTP:

```json
{
  "mcp": {
    "servers": {
      "penpot": {
        "url": "http://localhost:4401/mcp"
      }
    }
  }
}
```

#### Claude Desktop

O Claude Desktop exige o proxy `mcp-remote` (transporte apenas stdio):

1. Instale o proxy:

   ```bash
   npm install -g mcp-remote
   ```

2. Edite a configuracao do Claude Desktop:
   - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Windows**: `%APPDATA%/Claude/claude_desktop_config.json`
   - **Linux**: `~/.config/Claude/claude_desktop_config.json`

3. Adicione o server Penpot:

   ```json
   {
     "mcpServers": {
       "penpot": {
         "command": "npx",
         "args": ["-y", "mcp-remote", "http://localhost:4401/sse", "--allow-http"]
       }
     }
   }
   ```

4. **Feche totalmente** o Claude Desktop (File → Quit, nao apenas fechar a janela) e reinicie

#### Claude Code (CLI)

```bash
claude mcp add penpot -t http http://localhost:4401/mcp
```

## Solucao de Problemas

### Problemas de Conexao

#### "Plugin cannot connect to MCP server"

**Sintomas**: O plugin mostra "Not connected" mesmo apos clicar em Connect

**Solucoes**:

1. Verifique se os servers estao rodando:
   ```bash
   # Check if ports are in use
   lsof -i :4401  # MCP server
   lsof -i :4402  # WebSocket
   lsof -i :4400  # Plugin server
   ```

2. Reinicie os servers:

   ```bash
   # In the penpot-mcp directory
   npm run start:all
   ```

3. Verifique o console do browser (F12) para erros de WebSocket

#### Browser Bloqueia Conexao Local

**Sintomas**: O browser se recusa a conectar ao localhost a partir do Penpot

**Causa**: Chromium 142+ aplica restricoes de Private Network Access (PNA)

**Solucoes**:

1. **Chrome/Chromium**: Quando solicitado, permita acesso a rede local
2. **Brave**: Desative o Shield para o site do Penpot:
   - Clique no icone do Brave Shield na barra de enderecos
   - Desative o Shield para este site
3. **Tente Firefox**: O Firefox nao aplica essas restricoes com tanta rigidez

#### "WebSocket connection failed"

**Solucoes**:

1. Verifique configuracoes de firewall - libere portas 4400, 4401, 4402
2. Desative VPN se estiver ativa
3. Verifique aplicativos conflitantes usando as mesmas portas

### Problemas do MCP Client

#### Tools Nao Aparecem no VS Code/Claude

1. **Verifique o endpoint**:

   ```bash
   # Test the SSE endpoint
   curl http://localhost:4401/sse
   
   # Test the MCP endpoint
   curl http://localhost:4401/mcp
   ```

2. **Verifique a sintaxe de configuracao** - o JSON deve ser valido
3. **Reinicie o MCP client** completamente
4. **Verifique os logs do MCP server**:

   ```bash
   # Logs are in mcp-server/logs/
   tail -f mcp-server/logs/mcp-server.log
   ```

#### "Tool execution timed out"

**Causa**: Plugin desconectado ou operacao demorou demais

**Solucoes**:

1. Garanta que a UI do plugin ainda esteja aberta no Penpot
2. Verifique se o plugin mostra status "Connected"
3. Tente reconectar: clique em Disconnect e depois Connect no plugin

### Problemas do Plugin

#### "Plugin failed to load"

1. Verifique se o plugin server esta rodando na porta 4400
2. Tente acessar `http://localhost:4400/manifest.json` diretamente no browser
3. Limpe o cache do browser e recarregue o Penpot
4. Remova e adicione novamente o plugin

#### "Cannot find penpot object"

**Causa**: Plugin nao inicializado corretamente ou arquivo de design nao aberto

**Solucoes**:

1. Garanta que voce tenha um arquivo de design aberto (nao apenas o dashboard)
2. Aguarde alguns segundos apos abrir o arquivo antes de conectar
3. Atualize o Penpot e recarregue o plugin

### Problemas do Server

#### Porta ja em Uso

```bash
# Find process using the port
lsof -i :4401

# Kill the process if needed
kill -9 <PID>
```

Ou configure portas diferentes via variaveis de ambiente:
```bash
PENPOT_MCP_SERVER_PORT=4501 npm run start:all
```

#### Server Falha ao Iniciar

1. Verifique a versao do Node.js (deve ser v22+)
2. Apague `node_modules` e reinstale:

   ```bash
   rm -rf node_modules
   npm install
   npm run bootstrap
   ```

## Referencia de Configuracao

### Variaveis de Ambiente

| Variavel | Padrao | Descricao |
|----------|---------|-------------|
| `PENPOT_MCP_SERVER_PORT` | 4401 | HTTP/SSE server port |
| `PENPOT_MCP_WEBSOCKET_PORT` | 4402 | WebSocket server port |
| `PENPOT_MCP_SERVER_LISTEN_ADDRESS` | localhost | Endereco de bind do server |
| `PENPOT_MCP_LOG_LEVEL` | info | Log level (trace/debug/info/warn/error) |
| `PENPOT_MCP_LOG_DIR` | logs | Log file directory |
| `PENPOT_MCP_REMOTE_MODE` | false | Enable remote mode (disables file system access) |

### Exemplo: Configuracao Customizada

```bash
# Run on different ports with debug logging
PENPOT_MCP_SERVER_PORT=5000 \
PENPOT_MCP_WEBSOCKET_PORT=5001 \
PENPOT_MCP_LOG_LEVEL=debug \
npm run start:all
```

## Verificando o Setup

Execute este checklist para confirmar que tudo funciona:

1. **Servers Rodando**:
   ```bash
   curl -s http://localhost:4401/sse | head -1
   # Should return SSE stream headers
   ```

2. **Plugin Connected**: A UI do plugin mostra "Connected to MCP server"

3. **Tools Disponiveis**: No seu MCP client, verifique se estas tools aparecem:
   - `mcp__penpot__execute_code`
   - `mcp__penpot__export_shape`
   - `mcp__penpot__import_image`
   - `mcp__penpot__penpot_api_info`

4. **Execucao de Teste**: Peça ao seu assistente de IA para executar um comando simples:
   > "Use Penpot to get the current page name"

## Ajuda

- **GitHub Issues**: [penpot/penpot-mcp/issues](https://github.com/penpot/penpot-mcp/issues)
- **GitHub Discussions**: [penpot/penpot-mcp/discussions](https://github.com/penpot/penpot-mcp/discussions)
- **Penpot Community**: [community.penpot.app](https://community.penpot.app/)