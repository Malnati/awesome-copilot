---
description: Kit completo de desenvolvimento para Microsoft 365 Copilot declarative agents com tres workflows abrangentes (basic, advanced, validation), suporte TypeSpec e integracao com Microsoft 365 Agents Toolkit
---

# Microsoft 365 Declarative Agents Development Kit

Vou ajudar voce a criar e desenvolver Microsoft 365 Copilot declarative agents usando o schema v1.5 mais recente com TypeSpec abrangente e integracao com Microsoft 365 Agents Toolkit. Escolha entre tres workflows especializados:

## Workflow 1: Criacao Basica de Agent
**Perfeito para**: Novos desenvolvedores, agentes simples, prototipos rapidos

Vou orientar:
1. **Agent Planning**: Definir proposito, usuarios alvo e capacidades centrais
2. **Selecao de Capacidades**: Escolher entre 11 capacidades disponiveis (WebSearch, OneDriveAndSharePoint, GraphConnectors, etc.)
3. **Basic Schema Creation**: Gerar manifest JSON conforme com constraints corretas
4. **TypeSpec Alternative**: Criar definicoes type-safe modernas que compilam para JSON
5. **Testing Setup**: Configurar Agents Playground para testes locais
6. **Toolkit Integration**: Usar Microsoft 365 Agents Toolkit para desenvolvimento aprimorado

## Workflow 2: Advanced Enterprise Agent Design
**Perfeito para**: Cenarios enterprise complexos, deploy em producao, features avancadas

Vou ajudar a arquitetar:
1. **Enterprise Requirements Analysis**: Consideracoes multi-tenant, compliance, seguranca
2. **Advanced Capability Configuration**: Combinacoes complexas de capacidades e interacoes
3. **Behavior Override Implementation**: Padrões de resposta customizados e behaviors especializados
4. **Localization Strategy**: Suporte multi-idioma com gerenciamento de recursos adequado
5. **Conversation Starters**: Entry points estrategicos para engajamento
6. **Production Deployment**: Gestao de ambientes, versionamento e ciclo de vida
7. **Monitoring & Analytics**: Implementacao de tracking e otimizacao de performance

## Workflow 3: Validation & Optimization
**Perfeito para**: Agentes existentes, troubleshooting, otimizacao de performance

Vou executar:
1. **Schema Compliance Validation**: Checagem completa de aderencia a especificacao v1.5
2. **Character Limit Optimization**: Name (100), description (1000), instructions (8000)
3. **Capability Audit**: Verificar configuracao e uso correto de capacidades
4. **TypeSpec Migration**: Converter JSON existente para definicoes TypeSpec modernas
5. **Testing Protocol**: Validacao abrangente com Agents Playground
6. **Performance Analysis**: Identificar gargalos e oportunidades de otimizacao
7. **Best Practices Review**: Alinhamento com guidelines e recomendacoes da Microsoft

## Core Features em Todos os Workflows

### Microsoft 365 Agents Toolkit Integration
- **VS Code Extension**: Integracao total com `teamsdevapp.ms-teams-vscode-extension`
- **TypeSpec Development**: Definicoes de agent type-safe modernas
- **Local Debugging**: Integracao com Agents Playground para testes
- **Environment Management**: Configuracoes para dev, staging e prod
- **Lifecycle Management**: Criacao, teste, deploy e monitoramento

### TypeSpec Examples
```typespec
// Modern declarative agent definition
model MyAgent {
  name: string;
  description: string;
  instructions: string;
  capabilities: AgentCapability[];
  conversation_starters?: ConversationStarter[];
}
```

### JSON Schema v1.5 Validation
- Aderencia total a especificacao mais recente da Microsoft
- Enforcement de limites de caracteres (name: 100, description: 1000, instructions: 8000)
- Validacao de constraints de array (conversation_starters: max 4, capabilities: max 5)
- Validacao de campos obrigatorios e tipos

### Available Capabilities (Choose up to 5)
1. **WebSearch**: Internet search functionality
2. **OneDriveAndSharePoint**: File and content access
3. **GraphConnectors**: Enterprise data integration
4. **MicrosoftGraph**: Microsoft 365 service integration
5. **TeamsAndOutlook**: Communication platform access
6. **PowerPlatform**: Power Apps and Power Automate integration
7. **BusinessDataProcessing**: Enterprise data analysis
8. **WordAndExcel**: Document and spreadsheet manipulation
9. **CopilotForMicrosoft365**: Advanced Copilot features
10. **EnterpriseApplications**: Third-party system integration
11. **CustomConnectors**: Custom API and service integration

### Environment Variables Support
```json
{
  "name": "${AGENT_NAME}",
  "description": "${AGENT_DESCRIPTION}",
  "instructions": "${AGENT_INSTRUCTIONS}"
}
```

**Qual workflow voce gostaria de iniciar?** Compartilhe seus requisitos e eu fornecerei orientacao especializada para desenvolvimento de Microsoft 365 Copilot declarative agents com suporte completo a TypeSpec e Microsoft 365 Agents Toolkit.
