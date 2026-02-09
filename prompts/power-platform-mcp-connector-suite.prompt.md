---
description: Gere um conector customizado completo do Power Platform com integracao MCP para Copilot Studio - inclui geracao de schema, troubleshooting e validacao
agent: agent
---

# Suite de Conector MCP para Power Platform

Gere implementacoes completas de conectores customizados do Power Platform com integracao Model Context Protocol para Microsoft Copilot Studio.

## Capacidades MCP no Copilot Studio

**Atualmente Suportado:**
- ✅ **Tools**: Funcoes que o LLM pode chamar (com aprovacao do usuario)
- ✅ **Resources**: Dados tipo arquivo que agentes podem ler (devem ser outputs de tools)

**Ainda Nao Suportado:**
- ❌ **Prompts**: Templates pre-escritos (preparar para suporte futuro)

## Geracao de Connector

Crie um conector completo do Power Platform com:

**Arquivos Principais:**
- `apiDefinition.swagger.json` com `x-ms-agentic-protocol: mcp-streamable-1.0`
- `apiProperties.json` com metadados e autenticacao
- `script.csx` com transformacoes C# customizadas para tratamento MCP JSON-RPC
- `readme.md` com documentacao do connector

**Integracao MCP:**
- Endpoint POST `/mcp` para comunicacao JSON-RPC 2.0
- Definicoes de schema McpResponse e McpErrorResponse
- Compliance com restricoes do Copilot Studio (sem reference types, tipos unicos)
- Resources integrados como outputs de tools (Resources e Tools suportados; Prompts ainda nao)

## Validacao de Schema e Troubleshooting

**Valide schemas para compliance com Copilot Studio:**
- ✅ Sem reference types (`$ref`) em tool inputs/outputs
- ✅ Apenas valores de tipo unico (nao `['string', 'number']`)
- ✅ Tipos primitivos: string, number, integer, boolean, array, object
- ✅ Resources como outputs de tools, nao entidades separadas
- ✅ URIs completas em todos os endpoints

**Problemas comuns e correcoes:**
- Tools filtradas → Remova reference types, use primitivos
- Erros de tipo → Tipos unicos com logica de validacao
- Resources indisponiveis → Inclua em outputs de tools
- Falhas de conexao → Verifique header `x-ms-agentic-protocol`

## Variaveis de Contexto

- **Connector Name**: [Nome exibido do connector]
- **Server Purpose**: [O que o servidor MCP deve realizar]
- **Tools Needed**: [Lista de tools MCP a implementar]
- **Resources**: [Tipos de resources a fornecer]
- **Authentication**: [none, api-key, oauth2, basic]
- **Host Environment**: [Azure Function, Express.js, etc.]
- **Target APIs**: [APIs externas para integrar]

## Modos de Geracao

### Mode 1: Complete New Connector
Gere todos os arquivos para um novo conector MCP do Power Platform do zero, incluindo setup de validacao via CLI.

### Mode 2: Schema Validation
Analise e corrija schemas existentes para compliance com Copilot Studio usando paconn e ferramentas de validacao.

### Mode 3: Integration Troubleshooting
Diagnostique e resolva problemas de integracao MCP com Copilot Studio usando ferramentas de debug CLI.

### Mode 4: Hybrid Connector
Adicione capacidades MCP a um conector Power Platform existente com workflows de validacao adequados.

### Mode 5: Certification Preparation
Prepare o conector para submissao de certificacao da Microsoft com metadados completos e compliance de validacao.

### Mode 6: OAuth Security Hardening
Implemente autenticacao OAuth 2.0 com boas praticas de seguranca MCP e validacao avancada de token.

## Saida Esperada

**1. apiDefinition.swagger.json**
- Swagger 2.0 com extensoes Microsoft
- Endpoint MCP: `POST /mcp` com header de protocolo correto
- Definicoes de schema compativeis (apenas tipos primitivos)
- Definicoes McpResponse/McpErrorResponse

**2. apiProperties.json**
- Metadados e branding (`iconBrandColor` obrigatorio)
- Configuracao de autenticacao
- Policy templates para transformacoes MCP

**3. script.csx**
- Tratamento de mensagens JSON-RPC 2.0
- Transformacoes de request/response
- Logica de compliance do protocolo MCP
- Tratamento de erros e validacao

**4. Orientacao de implementacao**
- Padroes de registro e execucao de tools
- Estrategias de gerenciamento de resources
- Etapas de integracao com Copilot Studio
- Procedimentos de teste e validacao

## Checklist de Validacao

### Technical Compliance
- [ ] `x-ms-agentic-protocol: mcp-streamable-1.0` no endpoint MCP
- [ ] Sem reference types em qualquer definicao de schema
- [ ] Todos os campos type sao tipos unicos (nao arrays)
- [ ] Resources incluidos como outputs de tools
- [ ] Compliance JSON-RPC 2.0 em script.csx
- [ ] Endpoints de URI completa em todo o schema
- [ ] Descricoes claras para agentes do Copilot Studio
- [ ] Autenticacao configurada corretamente
- [ ] Policy templates para transformacoes MCP
- [ ] Compatibilidade com Generative Orchestration

### CLI Validation
- [ ] **paconn validate**: `paconn validate --api-def apiDefinition.swagger.json` passa sem erros
- [ ] **pac CLI ready**: Conector pode ser criado/atualizado com `pac connector create/update`
- [ ] **Script validation**: script.csx passa na validacao automatica durante upload no pac CLI
- [ ] **Package validation**: `ConnectorPackageValidator.ps1` executa com sucesso

### OAuth and Security Requirements
- [ ] **OAuth 2.0 Enhanced**: OAuth 2.0 padrao com boas praticas de seguranca MCP
- [ ] **Token Validation**: Validacao de audience para prevenir ataques de passthrough
- [ ] **Custom Security Logic**: Validacao aprimorada no script.csx para compliance MCP
- [ ] **State Parameter Protection**: Protecao de state parameter contra CSRF
- [ ] **HTTPS Enforcement**: Todos os endpoints de producao usam HTTPS
- [ ] **MCP Security Practices**: Prevencao de confused deputy attack em OAuth 2.0

### Certification Requirements
- [ ] **Complete metadata**: settings.json com informacoes de produto e servico
- [ ] **Icon compliance**: PNG, 230x230 ou 500x500
- [ ] **Documentation**: readme pronto para certificacao com exemplos abrangentes
- [ ] **Security compliance**: OAuth 2.0 com boas praticas MCP e politica de privacidade
- [ ] **Authentication flow**: OAuth 2.0 com validacao de seguranca customizada

## Exemplo de Uso

```yaml
Mode: Complete New Connector
Connector Name: Customer Analytics MCP
Server Purpose: Customer data analysis and insights
Tools Needed:
  - searchCustomers: Find customers by criteria
  - getCustomerProfile: Retrieve detailed customer data
  - analyzeCustomerTrends: Generate trend analysis
Resources:
  - Customer profiles (JSON data)
  - Analysis reports (structured data)
Authentication: oauth2
Host Environment: Azure Function
Target APIs: CRM REST API
```
