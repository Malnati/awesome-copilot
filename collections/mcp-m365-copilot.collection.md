# MCP-based M365 Agents Collection

Uma collection abrangente de prompts e instructions para construir agentes declarativos com integracao ao Model Context Protocol (MCP) para Microsoft 365 Copilot.

## Visao Geral

O Model Context Protocol (MCP) e um padrao universal que permite que modelos de AI integrem com sistemas externos por meio de endpoints de servidor padronizados. Esta collection fornece tudo que voce precisa para construir, implantar e gerenciar agentes declarativos baseados em MCP que estendem o Microsoft 365 Copilot com capacidades personalizadas.

## O que e Model Context Protocol?

MCP e um protocolo aberto desenvolvido para simplificar como modelos de AI se conectam a fontes de dados e tools externas. Em vez de codigo de integracao customizado para cada sistema, o MCP fornece uma interface consistente para:

- **Server Metadata**: Descobrir tools e capacidades disponiveis
- **Tools Listing**: Obter definicoes de funcoes e schemas
- **Tool Execution**: Invocar tools com parametros e receber resultados

Para Microsoft 365 Copilot, isso significa que voce pode criar agentes que se conectam a qualquer servidor compativel com MCP com configuracao point-and-click, sem escrever codigo customizado.

## Conteudo da Collection

### Prompts

1. **Create Declarative Agent** ([mcp-create-declarative-agent.prompt.md](../prompts/mcp-create-declarative-agent.prompt.md))
   - Construir agentes declarativos usando Microsoft 365 Agents Toolkit
   - Configurar integracao com servidor MCP com importacao de tools
   - Configurar autenticacao OAuth 2.0 ou SSO
   - Configurar response semantics para extracao de dados
   - Empacotar e implantar agents para testes

2. **Create Adaptive Cards** ([mcp-create-adaptive-cards.prompt.md](../prompts/mcp-create-adaptive-cards.prompt.md))
   - Projetar templates de Adaptive Card estaticos e dinamicos
   - Configurar response semantics (data_path, properties, template_selector)
   - Usar template language para condicionais e data binding
   - Criar cards responsivos que funcionam em diferentes superficies do Copilot
   - Implementar actions de card para interacoes de usuario

3. **Deploy and Manage Agents** ([mcp-deploy-manage-agents.prompt.md](../prompts/mcp-deploy-manage-agents.prompt.md))
   - Implantar agents via Microsoft 365 admin center
   - Configurar distribuicao organizacional ou store publica
   - Gerenciar ciclo de vida do agent (publish, deploy, block, remove)
   - Configurar governanca e controles de compliance
   - Monitorar uso e performance do agent

### Instructions

**MCP M365 Copilot Development Guidelines** ([mcp-m365-copilot.instructions.md](../instructions/mcp-m365-copilot.instructions.md))
- Boas praticas para design de MCP server e selecao de tools
- Organizacao de arquivos e estrutura de projeto
- Padroes de configuracao de response semantics
- Principios de design de Adaptive Card
- Requisitos de seguranca, governanca e compliance
- Workflows de teste e deploy

## Conceitos-Chave

### Agentes Declarativos

Agentes declarativos sao definidos por arquivos de configuracao, nao por codigo:
- **declarativeAgent.json**: Instrucoes do agent, capacidades, conversation starters
- **ai-plugin.json**: Tools do MCP server, response semantics, templates de adaptive card
- **mcp.json**: URL do MCP server, configuracao de autenticacao
- **manifest.json**: Teams app manifest para empacotamento

### Integracao com MCP Server

O Microsoft 365 Agents Toolkit fornece uma interface visual para:
1. **Scaffold** de um novo projeto de agent
2. **Add MCP action** para conectar a um servidor
3. **Choose tools** entre funcoes disponiveis no servidor
4. **Configure authentication** (OAuth 2.0, SSO)
5. **Generate files** (agent config, plugin manifest)
6. **Test** em m365.cloud.microsoft/chat

### Padroes de Autenticacao

**OAuth 2.0 Static Registration:**
- Pre-registrar app OAuth com o provedor de servico
- Armazenar credenciais em .env.local (nunca commitar)
- Referenciar em ai-plugin.json config de autenticacao
- Usuarios consentem uma vez, tokens armazenados no plugin vault

**Single Sign-On (SSO):**
- Usar Microsoft Entra ID para autenticacao
- Experiencia fluida para usuarios M365
- Sem necessidade de login separado
- Ideal para agentes internos organizacionais

### Response Semantics

Extraia e formate dados das respostas do MCP server:

```json
{
  "response_semantics": {
    "data_path": "$.items[*]",
    "properties": {
      "title": "$.name",
      "subtitle": "$.description",
      "url": "$.html_url"
    },
    "static_template": { ... }
  }
}
```

- **data_path**: JSONPath para extrair array ou objeto
- **properties**: Mapear campos de resposta para propriedades do Copilot
- **template_selector**: Escolher template dinamico com base na resposta
- **static_template**: Adaptive Card para formatacao visual

### Adaptive Cards

Respostas visuais ricas para outputs de agent:

**Static Templates:**
- Definidos uma vez em ai-plugin.json
- Usados para todas as respostas com a mesma estrutura
- Melhor performance e manutencao mais facil

**Dynamic Templates:**
- Retornados no corpo da resposta da API
- Selecionados via template_selector JSONPath
- Uteis para estruturas de resposta variadas

**Template Language:**
- `${property}`: Data binding
- `${if(condition, true, false)}`: Condicionais
- `${formatNumber(value, decimals)}`: Formatacao
- `$when`: Renderizacao condicional de elementos

## Opcoes de Deploy

### Organization Deployment
- Admin de TI implanta para todos os usuarios ou grupos especificos
- Requer aprovacao no Microsoft 365 admin center
- Melhor para agentes de negocio internos
- Controle total de governanca e compliance

### Agent Store
- Envio para Partner Center para validacao
- Disponibilidade publica para todos os usuarios do Copilot
- Revisao rigorosa de seguranca e compliance
- Adequado para agentes construidos por parceiros

## Exemplos de Parceiros

### monday.com
Integracao de gestao de tarefas e projetos:
- Criar tasks diretamente do Copilot
- Consultar status e atualizacoes do projeto
- Atribuir work items a membros da equipe
- Ver prazos e milestones

### Canva
Capacidades de automacao de design:
- Gerar conteudo com branding
- Criar graficos para redes sociais
- Acessar templates de design
- Exportar em multiplos formatos

### Sitecore
Integracao de gestao de conteudo:
- Buscar no repositorio de conteudo
- Criar e atualizar itens de conteudo
- Gerenciar workflows e aprovacoes
- Previsualizar conteudo em contexto

## Primeiros Passos

### Pre-requisitos
- Microsoft 365 Agents Toolkit extension (v6.3.x ou superior)
- Conta GitHub (para exemplos OAuth)
- Licenca do Microsoft 365 Copilot
- Acesso a um MCP server compativel

### Quick Start
1. Instale o Microsoft 365 Agents Toolkit no VS Code
2. Use o prompt **Create Declarative Agent** para fazer scaffolding do projeto
3. Adicione a URL do MCP server e escolha tools
4. Configure autenticacao com OAuth ou SSO
5. Use o prompt **Create Adaptive Cards** para projetar templates de resposta
6. Teste o agent em m365.cloud.microsoft/chat
7. Use o prompt **Deploy and Manage Agents** para distribuicao

### Development Workflow
```
1. Scaffold agent project
   ↓
2. Connect MCP server
   ↓
3. Import tools
   ↓
4. Configure authentication
   ↓
5. Design adaptive cards
   ↓
6. Test locally
   ↓
7. Deploy to organization
   ↓
8. Monitor and iterate
```

## Boas Praticas

### MCP Server Design
- Importe apenas tools necessarias (evite over-scoping)
- Use autenticacao segura (OAuth 2.0, SSO)
- Teste cada tool individualmente
- Valide que endpoints do servidor sao HTTPS
- Considere limites de token ao selecionar tools

### Agent Instructions
- Seja especifico e claro sobre capacidades do agent
- Forneca exemplos de como interagir
- Defina limites do que o agent pode ou nao pode fazer
- Use conversation starters para orientar usuarios

### Response Formatting
- Use JSONPath para extrair dados relevantes
- Mapeie propriedades claramente (title, subtitle, url)
- Projete adaptive cards para legibilidade
- Teste cards em diferentes superficies do Copilot (Chat, Teams, Outlook)

### Security and Governance
- Nunca commit credenciais no controle de versao
- Use variaveis de ambiente para secrets
- Siga o principio de menor privilegio
- Revise requisitos de compliance
- Monitore uso e performance do agent

## Casos de Uso Comuns

### Data Retrieval
- Buscar em sistemas externos
- Obter informacoes especificas do usuario
- Consultar bancos de dados ou APIs
- Agregar dados de multiplas fontes

### Task Automation
- Criar tickets ou tasks
- Atualizar registros ou status
- Disparar workflows
- Agendar acoes

### Content Generation
- Criar documentos ou designs
- Gerar relatorios ou resumos
- Formatar dados em templates
- Exportar em varios formatos

### Integration Scenarios
- Conectar sistemas de CRM
- Integrar tools de gestao de projetos
- Acessar knowledge bases
- Conectar apps de negocio customizados

## Troubleshooting

### Agent nao aparece no Copilot
- Verifique se o agent esta implantado no admin center
- Confirme que o usuario esta no grupo atribuido
- Confirme que o agent nao esta bloqueado
- Atualize a interface do Copilot

### Erros de Autenticacao
- Valide credenciais OAuth em .env.local
- Verifique se scopes correspondem as permissoes exigidas
- Teste o fluxo de auth de forma independente
- Verifique se o MCP server esta acessivel

### Problemas de Response Formatting
- Teste expressoes JSONPath com dados de exemplo
- Valide se data_path extrai o array/objeto esperado
- Verifique se mapeamentos de propriedades estao corretos
- Teste o adaptive card com diferentes estruturas de resposta

### Problemas de Performance
- Monitore tempos de resposta do MCP server
- Reduza o numero de tools importadas
- Otimize o tamanho dos dados de resposta
- Use caching quando apropriado

## Recursos

### Documentacao Oficial
- [Build Declarative Agents with MCP (DevBlogs)](https://devblogs.microsoft.com/microsoft365dev/build-declarative-agents-for-microsoft-365-copilot-with-mcp/)
- [Build MCP Plugins (Microsoft Learn)](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/build-mcp-plugins)
- [API Plugin Adaptive Cards (Microsoft Learn)](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/api-plugin-adaptive-cards)
- [Manage Copilot Agents (Microsoft Learn)](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/manage-copilot-agents-integrated-apps)

### Tools and Extensions
- [Microsoft 365 Agents Toolkit](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension)
- [Adaptive Cards Designer](https://adaptivecards.io/designer/)
- [Teams Toolkit](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/teams-toolkit-fundamentals)

### MCP Resources
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [MCP Server Directory](https://github.com/modelcontextprotocol/servers)
- Community MCP servers and examples

### Admin and Governance
- [Microsoft 365 Admin Center](https://admin.microsoft.com/)
- [Power Platform Admin Center](https://admin.powerplatform.microsoft.com/)
- [Partner Center](https://partner.microsoft.com/) for agent submissions

## Support and Community

- Participe da [Microsoft 365 Developer Community](https://developer.microsoft.com/en-us/microsoft-365/community)
- Faca perguntas no [Microsoft Q&A](https://learn.microsoft.com/en-us/answers/products/)
- Compartilhe feedback em [Microsoft 365 Copilot GitHub discussions](https://github.com/microsoft/copilot-feedback)

## What's Next?

Depois de dominar agentes baseados em MCP, explore:
- **Advanced tool composition**: Combine multiplos MCP servers
- **Custom authentication flows**: Implemente provedores OAuth customizados
- **Complex adaptive cards**: Cards multi-acao com dados dinamicos
- **Agent analytics**: Acompanhe padroes de uso e otimize
- **Multi-agent orchestration**: Construa agents que trabalham juntos

---

*Esta collection e mantida pela comunidade e reflete boas praticas atuais para desenvolvimento de agentes MCP-based M365 Copilot. Contribuicoes e feedback sao bem-vindos!*
