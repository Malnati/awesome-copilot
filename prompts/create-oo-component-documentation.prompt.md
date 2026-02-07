---
agent: 'agent'
description: 'Crie documentacao abrangente e padronizada para componentes orientados a objetos seguindo melhores praticas da industria e padroes de documentacao arquitetural.'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'extensions', 'web/fetch', 'githubRepo', 'openSimpleBrowser', 'problems', 'runTasks', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# Gerar Documentacao Padrao de Componente OO

Crie documentacao abrangente para o(s) componente(s) orientado(s) a objeto em: `${input:ComponentPath}`.

Analise o componente examinando o codigo no caminho fornecido. Se for uma pasta, analise todos os arquivos fonte. Se for um arquivo unico, trate como componente principal e analise arquivos relacionados no mesmo diretorio.

## Padroes de Documentacao

- DOC-001: Seguir niveis do modelo C4 (Context, Containers, Components, Code)
- DOC-002: Alinhar com o template Arc42 de documentacao de arquitetura de software
- DOC-003: Cumprir o padrao IEEE 1016 de Software Design Description
- DOC-004: Usar principios de Agile Documentation (documentacao suficiente que agrega valor)
- DOC-005: Direcionar para developers e maintainers como publico principal

## Instrucoes de Analise

- ANA-001: Determinar tipo de caminho (pasta vs arquivo unico) e identificar componente principal
- ANA-002: Examinar arquivos fonte para estruturas de classe e heranca
- ANA-003: Identificar design patterns e decisoes arquiteturais
- ANA-004: Documentar APIs publicas, interfaces e dependencias
- ANA-005: Reconhecer patterns criacionais/estruturais/comportamentais
- ANA-006: Documentar parametros, retornos e excecoes de metodos
- ANA-007: Avaliar performance, seguranca, confiabilidade e maintainability
- ANA-008: Inferir patterns de integracao e fluxo de dados

## Otimizacoes por Linguagem

- LNG-001: **C#/.NET** - async/await, dependency injection, configuracao, disposal
- LNG-002: **Java** - Spring framework, annotations, exception handling, packaging
- LNG-003: **TypeScript/JavaScript** - modules, async patterns, types, npm
- LNG-004: **Python** - packages, virtual environments, type hints, testing

## Tratamento de Erros

- ERR-001: Caminho nao existe - forneca orientacao de formato correto
- ERR-002: Nenhum arquivo fonte encontrado - sugira localizacoes alternativas
- ERR-003: Estrutura nao clara - documente achados e solicite esclarecimentos
- ERR-004: Patterns nao padrao - documente abordagens custom
- ERR-005: Codigo insuficiente - foque nas informacoes disponiveis e destaque lacunas

## Formato de Saida

Gere Markdown bem estruturado com hierarquia clara de headings, code blocks, tabelas, bullets e formatacao adequada para legibilidade e maintainability.

## Localizacao do Arquivo

A documentacao deve ser salva em `/docs/components/` e nomeada conforme a convencao: `[component-name]-documentation.md`.

## Estrutura de Documentacao Obrigatoria

O arquivo deve seguir o template abaixo, garantindo que todas as secoes sejam preenchidas. O front matter deve ser estruturado corretamente conforme o exemplo:

```md
---
title: [Component Name] - Technical Documentation
component_path: `${input:ComponentPath}`
version: [Optional: e.g., 1.0, Date]
date_created: [YYYY-MM-DD]
last_updated: [Optional: YYYY-MM-DD]
owner: [Optional: Team/Individual responsible for this component]
tags: [Optional: List of relevant tags or categories, e.g., `component`,`service`,`tool`,`infrastructure`,`documentation`,`architecture` etc]
---

# [Component Name] Documentation

[A short concise introduction to the component and its purpose within the system.]

## 1. Component Overview

### Purpose/Responsibility
- OVR-001: State component's primary responsibility
- OVR-002: Define scope (included/excluded functionality)
- OVR-003: Describe system context and relationships

## 2. Architecture Section

- ARC-001: Document design patterns used (Repository, Factory, Observer, etc.)
- ARC-002: List internal and external dependencies with purposes
- ARC-003: Document component interactions and relationships
- ARC-004: Include visual diagrams (UML class, sequence, component)
- ARC-005: Create mermaid diagram showing component structure, relationships, and dependencies

### Component Structure and Dependencies Diagram

Include a comprehensive mermaid diagram that shows:
- **Component structure** - Main classes, interfaces, and their relationships
- **Internal dependencies** - How components interact within the system
- **External dependencies** - External libraries, services, databases, APIs
- **Data flow** - Direction of dependencies and interactions
- **Inheritance/composition** - Class hierarchies and composition relationships

```mermaid
graph TD
    subgraph "Component System"
        A[Main Component] --> B[Internal Service]
        A --> C[Internal Repository]
        B --> D[Business Logic]
        C --> E[Data Access Layer]
    end

    subgraph "External Dependencies"
        F[External API]
        G[Database]
        H[Third-party Library]
        I[Configuration Service]
    end

    A --> F
    E --> G
    B --> H
    A --> I

    classDiagram
        class MainComponent {
            +property: Type
            +method(): ReturnType
            +asyncMethod(): Promise~Type~
        }
        class InternalService {
            +businessOperation(): Result
        }
        class ExternalAPI {
            <<external>>
            +apiCall(): Data
        }

        MainComponent --> InternalService
        MainComponent --> ExternalAPI
```

## 3. Interface Documentation

- INT-001: Document all public interfaces and usage patterns
- INT-002: Create method/property reference table
- INT-003: Document events/callbacks/notification mechanisms

| Method/Property | Purpose | Parameters | Return Type | Usage Notes |
|-----------------|---------|------------|-------------|-------------|
| [Name] | [Purpose] | [Parameters] | [Type] | [Notes] |

## 4. Implementation Details

- IMP-001: Document main implementation classes and responsibilities
- IMP-002: Describe configuration requirements and initialization
- IMP-003: Document key algorithms and business logic
- IMP-004: Note performance characteristics and bottlenecks

## 5. Usage Examples

### Basic Usage

```csharp
// Basic usage example
var component = new ComponentName();
component.DoSomething();
```

### Advanced Usage

```csharp
// Advanced configuration patterns
var options = new ComponentOptions();
var component = ComponentFactory.Create(options);
await component.ProcessAsync(data);
```

- USE-001: Provide basic usage examples
- USE-002: Show advanced configuration patterns
- USE-003: Document best practices and recommended patterns

## 6. Quality Attributes

- QUA-001: Security (authentication, authorization, data protection)
- QUA-002: Performance (characteristics, scalability, resource usage)
- QUA-003: Reliability (error handling, fault tolerance, recovery)
- QUA-004: Maintainability (standards, testing, documentation)
- QUA-005: Extensibility (extension points, customization options)

## 7. Reference Information

- REF-001: List dependencies with versions and purposes
- REF-002: Complete configuration options reference
- REF-003: Testing guidelines and mock setup
- REF-004: Troubleshooting (common issues, error messages)
- REF-005: Related documentation links
- REF-006: Change history and migration notes

```
