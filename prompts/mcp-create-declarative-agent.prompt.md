````prompt
---
mode: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Crie um agente declarativo para Microsoft 365 Copilot integrando um servidor MCP com autenticacao, selecao de tools e configuracao'
model: 'gpt-4.1'
tags: [mcp, m365-copilot, declarative-agent, model-context-protocol, api-plugin]
---

# Criar Agente Declarativo Baseado em MCP para Microsoft 365 Copilot

Crie um agente declarativo completo para Microsoft 365 Copilot que integra com um servidor Model Context Protocol (MCP) para acessar sistemas e dados externos.

## Requisitos

Gere a seguinte estrutura de projeto usando Microsoft 365 Agents Toolkit:

### Setup do Projeto
1. **Scaffold declarative agent** via Agents Toolkit
2. **Adicionar MCP action** apontando para o servidor MCP
3. **Selecionar tools** para importar do servidor MCP
4. **Configurar autenticacao** (OAuth 2.0 ou SSO)
5. **Revisar arquivos gerados** (manifest.json, ai-plugin.json, declarativeAgent.json)

### Arquivos-Chave Gerados

**appPackage/manifest.json** - Teams app manifest com referencia de plugin:
```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/teams/vDevPreview/MicrosoftTeams.schema.json",
  "manifestVersion": "devPreview",
  "version": "1.0.0",
  "id": "...",
  "developer": {
    "name": "...",
    "websiteUrl": "...",
    "privacyUrl": "...",
    "termsOfUseUrl": "..."
  },
  "name": {
    "short": "Agent Name",
    "full": "Full Agent Name"
  },
  "description": {
    "short": "Short description",
    "full": "Full description"
  },
  "copilotAgents": {
    "declarativeAgents": [
      {
        "id": "declarativeAgent",
        "file": "declarativeAgent.json"
      }
    ]
  }
}
```

**appPackage/declarativeAgent.json** - Definicao do agente:
```json
{
  "$schema": "https://aka.ms/json-schemas/copilot/declarative-agent/v1.0/schema.json",
  "version": "v1.0",
  "name": "Agent Name",
  "description": "Agent description",
  "instructions": "You are an assistant that helps with [specific domain]. Use the available tools to [capabilities].",
  "capabilities": [
    {
      "name": "WebSearch",
      "websites": [
        {
          "url": "https://learn.microsoft.com"
        }
      ]
    },
    {
      "name": "MCP",
      "file": "ai-plugin.json"
    }
  ]
}
```

**appPackage/ai-plugin.json** - MCP plugin manifest:
```json
{
  "schema_version": "v2.1",
  "name_for_human": "Service Name",
  "description_for_human": "Description for users",
  "description_for_model": "Description for AI model",
  "contact_email": "support@company.com",
  "namespace": "serviceName",
  "capabilities": {
    "conversation_starters": [
      {
        "text": "Example query 1"
      }
    ]
  },
  "functions": [
    {
      "name": "functionName",
      "description": "Function description",
      "capabilities": {
        "response_semantics": {
          "data_path": "$/",
          "properties": {
            "title": "$.title",
            "subtitle": "$.description"
          }
        }
      }
    }
  ],
  "runtimes": [
    {
      "type": "MCP",
      "spec": {
        "url": "https://api.service.com/mcp/"
      },
      "run_for_functions": ["functionName"],
      "auth": {
        "type": "OAuthPluginVault",
        "reference_id": "${{OAUTH_REFERENCE_ID}}"
      }
    }
  ]
}
```

**/.vscode/mcp.json** - Configuracao do servidor MCP:
```json
{
  "serverUrl": "https://api.service.com/mcp/",
  "pluginFilePath": "appPackage/ai-plugin.json"
}
```

## Integracao com Servidor MCP

### Endpoints MCP Suportados
O servidor MCP deve fornecer:
- Endpoint de **server metadata**
- Endpoint de **tools listing** (exibe funcoes disponiveis)
- Endpoint de **tool execution** (processa chamadas de funcao)

### Selecao de Tools
Ao importar do MCP:
1. Busque tools disponiveis no servidor
2. Selecione tools especificas para incluir (seguranca/simplicidade)
3. Definicoes de tools sao auto-geradas no ai-plugin.json

### Tipos de Autenticacao

**OAuth 2.0 (Static Registration)**
```json
"auth": {
  "type": "OAuthPluginVault",
  "reference_id": "${{OAUTH_REFERENCE_ID}}",
  "authorization_url": "https://auth.service.com/authorize",
  "client_id": "${{CLIENT_ID}}",
  "client_secret": "${{CLIENT_SECRET}}",
  "scope": "read write"
}
```

**Single Sign-On (SSO)**
```json
"auth": {
  "type": "SSO"
}
```

## Response Semantics

### Defina o Mapeamento de Dados
Use `response_semantics` para extrair campos relevantes das respostas da API:

```json
"capabilities": {
  "response_semantics": {
    "data_path": "$.results",
    "properties": {
      "title": "$.name",
      "subtitle": "$.description",
      "url": "$.link"
    }
  }
}
```

### Adicionar Adaptive Cards (Opcional)
Veja o prompt `mcp-create-adaptive-cards` para adicionar templates visuais.

## Configuracao de Ambiente

Crie `.env.local` ou `.env.dev` para credenciais:

```env
OAUTH_REFERENCE_ID=your-oauth-reference-id
CLIENT_ID=your-client-id
CLIENT_SECRET=your-client-secret
```

## Testing e Deploy

### Teste Local
1. **Provision** do agente no Agents Toolkit
2. **Start debugging** para sideload no Teams
3. Teste no Microsoft 365 Copilot em https://m365.cloud.microsoft/chat
4. Autentique quando solicitado
5. Consulte o agente usando linguagem natural

### Validacao
- Verifique imports de tools no ai-plugin.json
- Verifique configuracao de autenticacao
- Teste cada funcao exposta
- Valide mapeamento de dados de resposta

## Boas Praticas

### Tool Design
- **Funcoes focadas**: Cada tool deve fazer uma coisa bem
- **Descricoes claras**: Ajude o modelo a entender quando usar cada tool
- **Escopo minimo**: Importe apenas as tools que o agente precisa
- **Nomes descritivos**: Use nomes de funcao orientados a acao

### Security
- **Use OAuth 2.0** para cenarios de producao
- **Guarde secrets** em variaveis de ambiente
- **Valide inputs** no servidor MCP
- **Limite scopes** ao minimo necessario
- **Use reference IDs** para registro OAuth

### Instructions
- **Seja especifico** sobre o proposito e as capacidades do agente
- **Defina comportamento** para cenarios de sucesso e erro
- **Referencie tools** explicitamente nas instrucoes quando aplicavel
- **Defina expectativas** para usuarios sobre o que o agente pode/nao pode fazer

````
