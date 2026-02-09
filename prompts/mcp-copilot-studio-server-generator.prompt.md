---
description: Gere uma implementacao completa de servidor MCP otimizada para integracao com Copilot Studio, com restricoes de schema adequadas e suporte a HTTP streamable
agent: agent
---

# Gerador de Conector MCP para Power Platform

Gere um conector customizado do Power Platform com integracao Model Context Protocol (MCP) para Microsoft Copilot Studio. Este prompt cria todos os arquivos necessarios seguindo os padroes de connector da Power Platform com suporte a MCP streamable HTTP.

## Instrucoes

Crie uma implementacao completa de servidor MCP que:

1. **Usa o padrao MCP do Copilot Studio:**
   - Implementa `x-ms-agentic-protocol: mcp-streamable-1.0`
   - Suporta o protocolo de comunicacao JSON-RPC 2.0
   - Fornece endpoint HTTP streamable em `/mcp`
   - Segue a estrutura de connector da Power Platform

2. **Requisitos de Compliance do Schema:**
   - **Sem reference types** em inputs/outputs de tools (filtrados pelo Copilot Studio)
   - **Somente valores de tipo unico** (nao arrays de multiplos tipos)
   - **Evite inputs enum** (interpretados como string, nao enum)
   - Use tipos primitivos: string, number, integer, boolean, array, object
   - Garanta que todos os endpoints retornem URIs completas

3. **Componentes MCP a Incluir:**
   - **Tools**: Funcoes que o modelo de linguagem pode chamar (✅ Suportado no Copilot Studio)
   - **Resources**: Saidas tipo arquivo provenientes de tools (✅ Suportado no Copilot Studio - devem ser outputs de tools para ficarem acessiveis)
   - **Prompts**: Templates predefinidos para tarefas especificas (❌ Ainda nao suportado no Copilot Studio)

4. **Estrutura de Implementacao:**
   ```
   /apiDefinition.swagger.json  (Power Platform connector schema)
   /apiProperties.json         (Connector metadata and configuration)
   /script.csx                 (Custom code transformations and logic)
   /server/                    (MCP server implementation)
   /tools/                     (Individual MCP tools)
   /resources/                 (MCP resource handlers)
   ```

## Variaveis de Contexto

- **Server Purpose**: [Descreva o que o servidor MCP deve realizar]
- **Tools Needed**: [Lista de tools especificas para implementar]  
- **Resources**: [Tipos de resources a fornecer]
- **Authentication**: [Metodo de auth: none, api-key, oauth2]
- **Host Environment**: [Azure Function, Express.js, FastAPI, etc.]
- **Target APIs**: [APIs externas para integrar]

## Saida Esperada

Gere:

1. **apiDefinition.swagger.json** com:
   - `x-ms-agentic-protocol: mcp-streamable-1.0` correto
   - Endpoint MCP em POST `/mcp`
   - Definicoes de schema compativeis (sem reference types)
   - Definicoes McpResponse e McpErrorResponse

2. **apiProperties.json** com:
   - Metadados e branding do connector
   - Configuracao de autenticacao
   - Policy templates se necessario

3. **script.csx** com:
   - Codigo C# customizado para transformacoes de request/response
   - Logica de tratamento de mensagens MCP JSON-RPC
   - Funcoes de validacao e processamento
   - Tratamento de erros e logging

4. **Codigo do Servidor MCP** com:
   - Handler de request JSON-RPC 2.0
   - Registro e execucao de tools
   - Gerenciamento de resources (como outputs de tool)
   - Tratamento de erros adequado
   - Checks de compatibilidade com Copilot Studio

5. **Tools Individuais** que:
   - Aceitam apenas inputs de tipos primitivos
   - Retornam outputs estruturados
   - Incluem resources como outputs quando necessario
   - Fornecem descricoes claras para Copilot Studio

6. **Configuracao de Deploy** para:
   - Ambiente Power Platform
   - Integracao com agente do Copilot Studio
   - Testes e validacao

## Checklist de Validacao

Garanta que o codigo gerado:
- [ ] Nao tem reference types em schemas
- [ ] Todos os campos type sao tipos unicos
- [ ] Tratamento de enum via string com validacao
- [ ] Resources disponiveis via outputs de tool
- [ ] Endpoints de URI completa
- [ ] Compliance com JSON-RPC 2.0
- [ ] Header x-ms-agentic-protocol correto
- [ ] Schemas McpResponse/McpErrorResponse
- [ ] Descricoes claras de tool para Copilot Studio
- [ ] Compativel com Generative Orchestration

## Exemplo de Uso

```yaml
Server Purpose: Customer data management and analysis
Tools Needed: 
  - searchCustomers
  - getCustomerDetails
  - analyzeCustomerTrends
Resources:
  - Customer profiles
  - Analysis reports
Authentication: oauth2
Host Environment: Azure Function
Target APIs: CRM System REST API
```
