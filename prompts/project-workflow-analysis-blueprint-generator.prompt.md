---

description: 'Gerador de prompt abrangente e agnostico de tecnologia para documentar workflows end-to-end de aplicacoes. Detecta automaticamente padroes de arquitetura, stacks e fluxos de dados para gerar blueprints de implementacao detalhados cobrindo entry points, service layers, data access, error handling e abordagens de teste em multiplas tecnologias incluindo .NET, Java/Spring, React e arquiteturas de microservices.'

agent: 'agent'
---
# Gerador de Documentacao de Workflow de Projeto

## Variaveis de Configuracao

```
${PROJECT_TYPE="Auto-detect|.NET|Java|Spring|Node.js|Python|React|Angular|Microservices|Other"}
<!-- Primary technology stack -->

${ENTRY_POINT="API|GraphQL|Frontend|CLI|Message Consumer|Scheduled Job|Custom"}
<!-- Starting point for the flow -->

${PERSISTENCE_TYPE="Auto-detect|SQL Database|NoSQL Database|File System|External API|Message Queue|Cache|None"}
<!-- Data storage type -->

${ARCHITECTURE_PATTERN="Auto-detect|Layered|Clean|CQRS|Microservices|MVC|MVVM|Serverless|Event-Driven|Other"}
<!-- Primary architecture pattern -->

${WORKFLOW_COUNT=1-5}
<!-- Number of workflows to document -->

${DETAIL_LEVEL="Standard|Implementation-Ready"}
<!-- Level of implementation detail to include -->

${INCLUDE_SEQUENCE_DIAGRAM=true|false}
<!-- Generate sequence diagram -->

${INCLUDE_TEST_PATTERNS=true|false}
<!-- Include testing approach -->
```

## Prompt Gerado

```
"Analyze the codebase and document ${WORKFLOW_COUNT} representative end-to-end workflows 
that can serve as implementation templates for similar features. Use the following approach:
```

### Fase Inicial de Deteccao

```
${PROJECT_TYPE == "Auto-detect" ? 
  "Begin by examining the codebase structure to identify technologies:
   - Check for .NET solutions/projects, Spring configurations, Node.js/Express files, etc.
   - Identify the primary programming language(s) and frameworks in use
   - Determine the architectural patterns based on folder structure and key components" 
  : "Focus on ${PROJECT_TYPE} patterns and conventions"}
```

```
${ENTRY_POINT == "Auto-detect" ? 
  "Identify typical entry points by looking for:
   - API controllers or route definitions
   - GraphQL resolvers
   - UI components that initiate network requests
   - Message handlers or event subscribers
   - Scheduled job definitions" 
  : "Focus on ${ENTRY_POINT} entry points"}
```

```
${PERSISTENCE_TYPE == "Auto-detect" ? 
  "Determine persistence mechanisms by examining:
   - Database context/connection configurations
   - Repository implementations
   - ORM mappings
   - External API clients
   - File system interactions" 
  : "Focus on ${PERSISTENCE_TYPE} interactions"}
```

### Instrucoes de Documentacao de Workflow

Para cada um dos `${WORKFLOW_COUNT}` workflows mais representativos do sistema:

#### 1. Workflow Overview
   - Forneca um nome e uma breve descricao do workflow
   - Explique o proposito de negocio que ele atende
   - Identifique a acao ou evento disparador
   - Liste todos os arquivos/classes envolvidos no workflow completo

#### 2. Entry Point Implementation

**API Entry Points:**
```
${ENTRY_POINT == "API" || ENTRY_POINT == "Auto-detect" ? 
  "- Document the API controller class and method that receives the request
   - Show the complete method signature including attributes/annotations
   - Include the full request DTO/model class definition
   - Document validation attributes and custom validators
   - Show authentication/authorization attributes and checks" : ""}
```

**GraphQL Entry Points:**
```
${ENTRY_POINT == "GraphQL" || ENTRY_POINT == "Auto-detect" ? 
  "- Document the GraphQL resolver class and method
   - Show the complete schema definition for the query/mutation
   - Include input type definitions
   - Show resolver method implementation with parameter handling" : ""}
```

**Frontend Entry Points:**
```
${ENTRY_POINT == "Frontend" || ENTRY_POINT == "Auto-detect" ? 
  "- Document the component that initiates the API call
   - Show the event handler that triggers the request
   - Include the API client service method
   - Show state management code related to the request" : ""}
```

**Message Consumer Entry Points:**
```
${ENTRY_POINT == "Message Consumer" || ENTRY_POINT == "Auto-detect" ? 
  "- Document the message handler class and method
   - Show message subscription configuration
   - Include the complete message model definition
   - Show deserialization and validation logic" : ""}
```

#### 3. Service Layer Implementation
   - Documente cada classe de service envolvida com suas dependencias
   - Mostre assinaturas completas de metodo com parametros e tipos de retorno
   - Inclua implementacoes reais com logica de negocio chave
   - Documente definicoes de interface quando aplicavel
   - Mostre padroes de registro de dependency injection

**CQRS Patterns:**
```
${ARCHITECTURE_PATTERN == "CQRS" || ARCHITECTURE_PATTERN == "Auto-detect" ? 
  "- Include complete command/query handler implementations" : ""}
```

**Clean Architecture Patterns:**
```
${ARCHITECTURE_PATTERN == "Clean" || ARCHITECTURE_PATTERN == "Auto-detect" ? 
  "- Show use case/interactor implementations" : ""}
```

#### 4. Data Mapping Patterns
   - Documente codigo de mapeamento de DTO para domain model
   - Mostre configuracoes de object mapper ou metodos de mapeamento manual
   - Inclua logica de validacao durante o mapeamento
   - Documente domain events criados durante o mapeamento

#### 5. Data Access Implementation
   - Documente interfaces de repository e suas implementacoes
   - Mostre assinaturas completas de metodo com parametros e tipos de retorno
   - Inclua implementacoes reais de queries
   - Documente definicoes de entity/model com todas as propriedades
   - Mostre padroes de tratamento de transacao

**SQL Database Patterns:**
```
${PERSISTENCE_TYPE == "SQL Database" || PERSISTENCE_TYPE == "Auto-detect" ? 
  "- Include ORM configurations, annotations, or Fluent API usage
   - Show actual SQL queries or ORM statements" : ""}
```

**NoSQL Database Patterns:**
```
${PERSISTENCE_TYPE == "NoSQL Database" || PERSISTENCE_TYPE == "Auto-detect" ? 
  "- Show document structure definitions
   - Include document query/update operations" : ""}
```

#### 6. Response Construction
   - Documente definicoes de DTO/model de resposta
   - Mostre mapeamento de domain/entity para response
   - Inclua logica de selecao de status code
   - Documente estrutura de error response e geracao

#### 7. Error Handling Patterns
   - Documente tipos de excecao usados no workflow
   - Mostre padroes try/catch em cada camada
   - Inclua configuracoes de global exception handler
   - Documente implementacoes de error logging
   - Mostre politicas de retry ou circuit breaker
   - Inclua acoes compensatorias para cenarios de falha

#### 8. Asynchronous Processing Patterns
   - Documente codigo de agendamento de background jobs
   - Mostre implementacoes de publicacao de eventos
   - Inclua padroes de envio para message queues
   - Documente implementacoes de callback ou webhook
   - Mostre como operacoes async sao rastreadas e monitoradas

**Testing Approach (Optional):**
```
${INCLUDE_TEST_PATTERNS ? 
  "9. **Testing Approach**
     - Document unit test implementations for each layer
     - Show mocking patterns and test fixture setup
     - Include integration test implementations
     - Document test data generation approaches
     - Show API/controller test implementations" : ""}
```

**Sequence Diagram (Optional):**
```
${INCLUDE_SEQUENCE_DIAGRAM ? 
  "10. **Sequence Diagram**
      - Generate a detailed sequence diagram showing all components
      - Include method calls with parameter types
      - Show return values between components
      - Document conditional flows and error paths" : ""}
```

#### 11. Naming Conventions
Documente padroes consistentes para:
- Nomenclatura de controllers (por exemplo, `EntityNameController`)
- Nomenclatura de services (por exemplo, `EntityNameService`)
- Nomenclatura de repositories (por exemplo, `IEntityNameRepository`)
- Nomenclatura de DTOs (por exemplo, `EntityNameRequest`, `EntityNameResponse`)
- Padroes de nomenclatura de metodos para operacoes CRUD
- Convencoes de nomenclatura de variaveis
- Padroes de organizacao de arquivos

#### 12. Implementation Templates
Forneca templates reutilizaveis para:
- Criar um novo endpoint de API seguindo o padrao
- Implementar um novo metodo de service
- Adicionar um novo metodo de repository
- Criar novas classes de domain model
- Implementar error handling adequado

### Padroes de Implementacao Especificos por Tecnologia

**.NET Implementation Patterns (if detected):**
```
${PROJECT_TYPE == ".NET" || PROJECT_TYPE == "Auto-detect" ? 
  "- Complete controller class with attributes, filters, and dependency injection
   - Service registration in Startup.cs or Program.cs
   - Entity Framework DbContext configuration
   - Repository implementation with EF Core or Dapper
   - AutoMapper profile configurations
   - Middleware implementations for cross-cutting concerns
   - Extension method patterns
   - Options pattern implementation for configuration
   - Logging implementation with ILogger
   - Authentication/authorization filter or policy implementations" : ""}
```

**Spring Implementation Patterns (if detected):**
```
${PROJECT_TYPE == "Java" || PROJECT_TYPE == "Spring" || PROJECT_TYPE == "Auto-detect" ? 
  "- Complete controller class with annotations and dependency injection
   - Service implementation with transaction boundaries
   - Repository interface and implementation
   - JPA entity definitions with relationships
   - DTO class implementations
   - Bean configuration and component scanning
   - Exception handler implementations
   - Custom validator implementations" : ""}
```

**React Implementation Patterns (if detected):**
```
${PROJECT_TYPE == "React" || PROJECT_TYPE == "Auto-detect" ? 
  "- Component structure with props and state
   - Hook implementation patterns (useState, useEffect, custom hooks)
   - API service implementation
   - State management patterns (Context, Redux)
   - Form handling implementations
   - Route configuration" : ""}
```

### Diretrizes de Implementacao

Com base nos workflows documentados, forneca orientacao especifica para implementar novas features:

#### 1. Processo de Implementacao Passo a Passo
- Onde comecar ao adicionar uma feature similar
- Ordem de implementacao (por exemplo, model → repository → service → controller)
- Como integrar com cross-cutting concerns existentes

#### 2. Common Pitfalls to Avoid
- Identifique areas propensas a erros na implementacao atual
- Observe consideracoes de performance
- Liste bugs ou issues comuns encontrados

#### 3. Mecanismos de Extensao
- Documente como integrar com extension points existentes
- Mostre como adicionar novo comportamento sem modificar codigo existente
- Explique padroes de feature baseada em configuracao

**Conclusao:**
Conclua com um resumo dos padroes mais importantes que devem ser seguidos ao 
implementar novas features para manter consistencia com o codebase."
