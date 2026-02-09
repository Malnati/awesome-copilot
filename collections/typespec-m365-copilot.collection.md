# TypeSpec for Microsoft 365 Copilot

## Visao Geral

TypeSpec for Microsoft 365 Copilot e uma linguagem especifica de dominio (DSL) poderosa que permite aos desenvolvedores criar agentes declarativos e API plugins usando uma sintaxe limpa e expressiva. Construida sobre a base do [TypeSpec](https://typespec.io/), essa linguagem especializada fornece decorators e capacidades especificas do Microsoft 365 que simplificam o processo de desenvolvimento para estender o Microsoft 365 Copilot.

## Por que usar TypeSpec?

- **Type Safety**: Checagem de tipos abrangente para todos os constructs especificos do Microsoft 365 Copilot
- **Developer Experience**: Suporte rico de IntelliSense no Visual Studio Code com feedback em tempo real
- **Simplified Authoring**: Substitui configuracoes JSON verbosas por sintaxe intuitiva baseada em decorators
- **Automatic Manifest Generation**: Gera automaticamente arquivos de manifest validos e especificacoes OpenAPI
- **Maintainability**: Codebase mais legivel e manutenivel em comparacao com autoracao manual em JSON

## Conceitos Centrais

### Agentes Declarativos

Um agente declarativo e uma versao personalizada do Microsoft 365 Copilot que permite aos usuarios criar experiencias personalizadas declarando instrucoes, acoes e conhecimento especificos.

**Exemplo basico de agent:**
```typescript
@agent(
  "Customer Support Assistant",
  "An AI agent that helps with customer support inquiries and ticket management"
)
@instructions("""
  You are a customer support specialist. Help users with their inquiries,
  provide troubleshooting steps, and escalate complex issues when necessary.
  Always maintain a helpful and professional tone.
""")
@conversationStarter(#{
  title: "Check Ticket Status",
  text: "What's the status of my support ticket?"
})
namespace CustomerSupportAgent {
  // Agent capabilities defined here
}
```

### API Plugins

API plugins estendem o Microsoft 365 Copilot com operacoes de API customizadas, permitindo integracao com servicos externos e fontes de dados.

**Exemplo basico de API plugin:**
```typescript
import "@typespec/http";
import "@microsoft/typespec-m365-copilot";

using TypeSpec.Http;
using Microsoft.M365Copilot;

@service
@server("https://api.contoso.com")
@actions(#{
  nameForHuman: "Project Management API",
  descriptionForHuman: "Manage projects and tasks",
  descriptionForModel: "API for creating, updating, and tracking project tasks"
})
namespace ProjectAPI {
  model Project {
    id: string;
    name: string;
    description?: string;
    status: "active" | "completed" | "on-hold";
    createdDate: utcDateTime;
  }

  @route("/projects")
  @get op listProjects(): Project[];

  @route("/projects/{id}")
  @get op getProject(@path id: string): Project;

  @route("/projects")
  @post op createProject(@body project: CreateProjectRequest): Project;
}
```

## Decorators Principais

### Decorators de Agent

- **@agent**: Define um agent com nome, descricao e ID opcional
- **@instructions**: Define instrucoes comportamentais e diretrizes para o agent
- **@conversationStarter**: Define prompts iniciais de conversa para usuarios
- **@behaviorOverrides**: Modifica configuracoes de comportamento de orquestracao do agent
- **@disclaimer**: Exibe avisos legais ou de compliance para usuarios
- **@customExtension**: Adiciona pares chave-valor customizados para extensibilidade

### Decorators de API Plugin

- **@actions**: Define metadados de action incluindo nomes, descricoes e URLs
- **@authReferenceId**: Especifica ID de referencia de autenticacao para acesso a API
- **@capabilities**: Configura capacidades de funcao como confirmacoes e formatacao de resposta
- **@card**: Define templates de Adaptive Card para respostas de funcao
- **@reasoning**: Fornece instrucoes de raciocinio para invocacao de funcao
- **@responding**: Define instrucoes de formatacao de resposta para funcoes

## Capacidades do Agent

TypeSpec fornece capacidades embutidas para acessar servicos do Microsoft 365 e recursos externos:

### Knowledge Sources

**Web Search**
```typescript
op webSearch is AgentCapabilities.WebSearch<Sites = [
  {
    url: "https://learn.microsoft.com"
  }
]>;
```

**OneDrive and SharePoint**
```typescript
op oneDriveAndSharePoint is AgentCapabilities.OneDriveAndSharePoint<
  ItemsByUrl = [
    { url: "https://contoso.sharepoint.com/sites/ProductSupport" }
  ]
>;
```

**Teams Messages**
```typescript
op teamsMessages is AgentCapabilities.TeamsMessages<Urls = [
  {
    url: "https://teams.microsoft.com/l/team/...",
  }
]>;
```

**Email**
```typescript
op email is AgentCapabilities.Email<Folders = [
  {
    folderId: "Inbox",
  }
]>;
```

**People**
```typescript
op people is AgentCapabilities.People;
```

**Copilot Connectors**
```typescript
op copilotConnectors is AgentCapabilities.GraphConnectors<Connections = [
  {
    connectionId: "policieslocal",
  }
]>;
```

**Dataverse**
```typescript
op dataverse is AgentCapabilities.Dataverse<KnowledgeSources = [
  {
    hostName: "contoso.crm.dynamics.com";
    tables: [
      { tableName: "account" },
      { tableName: "contact" }
    ];
  }
]>;
```

### Productivity Tools

**Code Interpreter**
```typescript
op codeInterpreter is AgentCapabilities.CodeInterpreter;
```

**Image Generator**
```typescript
op graphicArt is AgentCapabilities.GraphicArt;
```

**Meetings**
```typescript
op meetings is AgentCapabilities.Meetings;
```

**Scenario Models**
```typescript
op scenarioModels is AgentCapabilities.ScenarioModels<ModelsById = [
  { id: "financial-forecasting-model-v3" }
]>;
```

## Autenticacao

TypeSpec suporta multiplos metodos de autenticacao para proteger API plugins:

### Sem Autenticacao (Anonymous)
```typescript
@service
@actions(ACTIONS_METADATA)
@server(SERVER_URL, API_NAME)
namespace API {
  // Endpoints
}
```

### Autenticacao por API Key
```typescript
@service
@actions(ACTIONS_METADATA)
@server(SERVER_URL, API_NAME)
@useAuth(ApiKeyAuth<ApiKeyLocation.header, "X-Your-Key">)
namespace API {
  // Endpoints
}
```

### OAuth2 Authorization Code Flow
```typescript
@service
@actions(ACTIONS_METADATA)
@server(SERVER_URL, API_NAME)
@useAuth(OAuth2Auth<[{ 
  type: OAuth2FlowType.authorizationCode;
  authorizationUrl: "https://contoso.com/oauth2/v2.0/authorize";
  tokenUrl: "https://contoso.com/oauth2/v2.0/token";
  refreshUrl: "https://contoso.com/oauth2/v2.0/token";
  scopes: ["scope-1", "scope-2"];
}]>)
namespace API {
  // Endpoints
}
```

### Usando Autenticacao Registrada
```typescript
@authReferenceId("NzFmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IyM5NzQ5Njc3Yi04NDk2LTRlODYtOTdmZS1kNDUzODllZjUxYjM=")
model Auth is OAuth2Auth<[{ 
  type: OAuth2FlowType.authorizationCode;
  authorizationUrl: "https://contoso.com/oauth2/v2.0/authorize";
  tokenUrl: "https://contoso.com/oauth2/v2.0/token";
  refreshUrl: "https://contoso.com/oauth2/v2.0/token";
  scopes: ["scope-1", "scope-2"];
}]>
```

## Cenarios Comuns

### Multi-Capability Knowledge Worker Agent
```typescript
import "@typespec/http";
import "@typespec/openapi3";
import "@microsoft/typespec-m365-copilot";

using TypeSpec.Http;
using TypeSpec.M365.Copilot.Agents;

@agent({
  name: "Knowledge Worker Assistant",
  description: "An intelligent assistant that helps with research, file management, and finding colleagues"
})
@instructions("""
  You are a knowledgeable research assistant specialized in helping knowledge workers
  find information efficiently. You can search the web for external research, access
  SharePoint documents for organizational content, and help locate colleagues within
  the organization.
""")
namespace KnowledgeWorkerAgent {
  op webSearch is AgentCapabilities.WebSearch<Sites = [
    {
      url: "https://learn.microsoft.com";
    }
  ]>;

  op oneDriveAndSharePoint is AgentCapabilities.OneDriveAndSharePoint<
    ItemsByUrl = [
      { url: "https://contoso.sharepoint.com/sites/IT" }
    ]
  >;

  op people is AgentCapabilities.People;
}
```

### API Plugin com Autenticacao
```typescript
import "@typespec/http";
import "@microsoft/typespec-m365-copilot";

using TypeSpec.Http;
using TypeSpec.M365.Copilot.Actions;

@service
@actions(#{
    nameForHuman: "Repairs Hub API",
    descriptionForModel: "Comprehensive repair management system",
    descriptionForHuman: "Manage facility repairs and track assignments"
})
@server("https://repairshub-apikey.contoso.com", "Repairs Hub API")
@useAuth(RepairsHubApiKeyAuth)
namespace RepairsHub {
  @route("/repairs")
  @get
  @action
  @card(#{
    dataPath: "$.",
    title: "$.title",
    url: "$.image",
    file: "cards/card.json"
  })
  op listRepairs(
    @query assignedTo?: string
  ): string;

  @route("/repairs")
  @post
  @action
  @capabilities(#{
    confirmation: #{
      type: "AdaptiveCard",
      title: "Create a new repair",
      body: """
      Creating a new repair with the following details:
        * **Title**: {{ function.parameters.title }}
        * **Description**: {{ function.parameters.description }}
      """
    }
  })
  op createRepair(
    @body repair: Repair
  ): Repair;

  model Repair {
    id?: string;
    title: string;
    description?: string;
    assignedTo?: string;
  }

  @authReferenceId("${{REPAIRSHUBAPIKEYAUTH_REFERENCE_ID}}")
  model RepairsHubApiKeyAuth is ApiKeyAuth<ApiKeyLocation.query, "code">;
}
```

## Primeiros Passos

### Pre-requisitos
- [Visual Studio Code](https://code.visualstudio.com/)
- [Microsoft 365 Agents Toolkit Visual Studio Code extension](https://aka.ms/M365AgentsToolkit)
- Licenca do Microsoft 365 Copilot

### Crie seu primeiro Agent

1. Abra o Visual Studio Code
2. Selecione **Microsoft 365 Agents Toolkit > Create a New Agent/App**
3. Selecione **Declarative Agent**
4. Selecione **Start with TypeSpec for Microsoft 365 Copilot**
5. Escolha a localizacao e o nome do projeto
6. Edite o arquivo `main.tsp` para personalizar seu agent
7. Selecione **Provision** no painel Lifecycle para fazer o deploy

## Boas Praticas

### Instructions
- Seja especifico e claro sobre o papel e a especialidade do agent
- Defina comportamentos a evitar, bem como comportamentos desejados
- Mantenha instructions abaixo de 8.000 caracteres
- Use strings com aspas triplas para instructions multilinha

### Conversation Starters
- Forneca 2-4 exemplos diversos de como interagir com o agent
- Torne-os especificos para as capacidades do agent
- Mantenha titulos concisos (menos de 100 caracteres)

### Capabilities
- Inclua apenas as capacidades de que seu agent realmente precisa
- Delimite capacidades a recursos especificos quando possivel
- Use URLs e IDs para limitar acesso a conteudo relevante

### API Operations
- Use nomes descritivos para operacoes e parametros com nomes claros
- Forneca descricoes detalhadas para consumidores humanos e de modelo
- Use dialogs de confirmacao para operacoes destrutivas
- Implemente tratamento de erros adequado com mensagens de erro significativas

### Authentication
- Use configuracoes de autenticacao registradas para producao
- Siga o principio de menor privilegio para scopes
- Armazene credenciais sensiveis em variaveis de ambiente
- Use `@authReferenceId` para referenciar configuracoes registradas

## Fluxo de Desenvolvimento

1. **Create**: Use o Microsoft 365 Agents Toolkit para fazer scaffolding do projeto
2. **Define**: Escreva suas definicoes TypeSpec em `main.tsp` e `actions.tsp`
3. **Configure**: Configure autenticacao e capacidades
4. **Provision**: Faca o deploy no seu ambiente de desenvolvimento
5. **Test**: Valide no Microsoft 365 Copilot (https://m365.cloud.microsoft/chat)
6. **Debug**: Use o Copilot developer mode para troubleshooting
7. **Iterate**: Refine com base no feedback de testes
