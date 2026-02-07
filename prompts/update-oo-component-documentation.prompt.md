---
agent: 'agent'
description: 'Atualize a documentacao existente de componentes orientados a objetos seguindo as melhores praticas da industria e padroes de documentacao de arquitetura.'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'extensions', 'web/fetch', 'githubRepo', 'openSimpleBrowser', 'problems', 'runTasks', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# Atualizar Documentacao Padrao de Componente OO

Atualize o arquivo de documentacao existente em: `${file}` analisando o codigo do componente correspondente.

Extraia o caminho do componente a partir do front matter da documentacao existente (campo `component_path`) ou infira-o a partir do conteudo da documentacao. Analise a implementacao atual do componente e atualize a documentacao de acordo.

**Padroes de Documentacao:**

- DOC-001: Siga os niveis de documentacao do Modelo C4 (Context, Containers, Components, Code)
- DOC-002: Alinhe com o template de documentacao de arquitetura de software Arc42
- DOC-003: Cumpra o padrao IEEE 1016 Software Design Description
- DOC-004: Use principios de Documentacao Agil (documentacao suficiente que agrega valor)
- DOC-005: Direcione desenvolvedores e mantenedores como publico principal

**Instrucoes de Analise:**

- ANA-001: Leia a documentacao existente para entender o contexto e a estrutura do componente
- ANA-002: Identifique o caminho do componente a partir do front matter ou da analise de conteudo
- ANA-003: Examine os arquivos de codigo-fonte atuais para estruturas de classes e heranca
- ANA-004: Compare a documentacao existente com a implementacao atual
- ANA-005: Identifique padroes de design e mudancas arquiteturais
- ANA-006: Atualize APIs publicas, interfaces e dependencias
- ANA-007: Reconheca novos/alterados padroes criacionais/estruturais/comportamentais
- ANA-008: Atualize parametros de metodo, valores de retorno, excecoes
- ANA-009: Reavalie performance, seguranca, confiabilidade, manutenibilidade
- ANA-010: Atualize padroes de integracao e fluxo de dados

**Otimizacoes Especificas de Linguagem:**

- LNG-001: **C#/.NET** - async/await, dependency injection, configuration, disposal
- LNG-002: **Java** - Spring framework, annotations, exception handling, packaging
- LNG-003: **TypeScript/JavaScript** - modules, async patterns, types, npm
- LNG-004: **Python** - packages, virtual environments, type hints, testing

**Estrategia de Atualizacao:**

- UPD-001: Preserve a estrutura e o formato da documentacao existente
- UPD-002: Atualize o campo `last_updated` para a data atual
- UPD-003: Mantenha o historico de versoes no front matter, se presente
- UPD-004: Adicione novas secoes se o componente tiver se expandido significativamente
- UPD-005: Marque funcionalidades obsoletas ou mudancas com quebra
- UPD-006: Atualize exemplos para refletir a API atual
- UPD-007: Atualize listas de dependencias e versoes
- UPD-008: Atualize diagramas mermaid para refletir a arquitetura atual

**Tratamento de Erros:**

- ERR-001: Arquivo de documentacao nao existe - forneca orientacao sobre o local do arquivo
- ERR-002: Caminho do componente nao encontrado na documentacao - solicite esclarecimento
- ERR-003: Codigo-fonte foi movido - sugira caminhos atualizados
- ERR-004: Grandes mudancas arquiteturais - destaque mudancas com quebra
- ERR-005: Acesso insuficiente ao codigo-fonte - documente limitacoes

**Formato de Saida:**

Atualize o arquivo Markdown existente mantendo sua estrutura enquanto atualiza o conteudo para corresponder a implementacao atual. Preserve formatacao, hierarquia de cabecalhos e decisoes organizacionais existentes.

**Estrutura de Documentacao Obrigatoria:**

Atualize a documentacao existente seguindo a mesma estrutura de template, garantindo que todas as secoes reflitam a implementacao atual:

```md
---
title: [Component Name] - Technical Documentation
component_path: [Current component path]
version: [Updated version if applicable]
date_created: [Original creation date - preserve]
last_updated: [YYYY-MM-DD - update to current date]
owner: [Preserve existing or update if changed]
tags: [Update tags as needed based on current functionality]
---

# [Component Name] Documentation

[Update introduction to reflect current component purpose and capabilities]

## 1. Component Overview

### Purpose/Responsibility
- OVR-001: Update component's primary responsibility
- OVR-002: Refresh scope (included/excluded functionality)
- OVR-003: Update system context and relationships

## 2. Architecture Section

- ARC-001: Update design patterns used (Repository, Factory, Observer, etc.)
- ARC-002: Refresh internal and external dependencies with current purposes
- ARC-003: Update component interactions and relationships
- ARC-004: Update visual diagrams (UML class, sequence, component)
- ARC-005: Refresh mermaid diagram showing current component structure, relationships, and dependencies

### Component Structure and Dependencies Diagram

Update the mermaid diagram to show current:
- **Component structure** - Current classes, interfaces, and their relationships
- **Internal dependencies** - How components currently interact within the system
- **External dependencies** - Current external libraries, services, databases, APIs
- **Data flow** - Current direction of dependencies and interactions
- **Inheritance/composition** - Current class hierarchies and composition relationships

```mermaid
[Update diagram to reflect current architecture]
```

## 3. Interface Documentation

- INT-001: Update all public interfaces and current usage patterns
- INT-002: Refresh method/property reference table with current API
- INT-003: Update events/callbacks/notification mechanisms

| Method/Property | Purpose | Parameters | Return Type | Usage Notes |
|-----------------|---------|------------|-------------|-------------|
| [Update table with current API] | | | | |

## 4. Implementation Details

- IMP-001: Update main implementation classes and current responsibilities
- IMP-002: Refresh configuration requirements and initialization patterns
- IMP-003: Update key algorithms and business logic
- IMP-004: Update performance characteristics and bottlenecks

## 5. Usage Examples

### Basic Usage

```csharp
// Update basic usage example to current API
```

### Advanced Usage

```csharp
// Update advanced configuration patterns to current implementation
```

- USE-001: Update basic usage examples
- USE-002: Refresh advanced configuration patterns
- USE-003: Update best practices and recommended patterns

## 6. Quality Attributes

- QUA-001: Update security (authentication, authorization, data protection)
- QUA-002: Refresh performance (characteristics, scalability, resource usage)
- QUA-003: Update reliability (error handling, fault tolerance, recovery)
- QUA-004: Refresh maintainability (standards, testing, documentation)
- QUA-005: Update extensibility (extension points, customization options)

## 7. Reference Information

- REF-001: Update dependencies with current versions and purposes
- REF-002: Refresh configuration options reference
- REF-003: Update testing guidelines and mock setup
- REF-004: Refresh troubleshooting (common issues, error messages)
- REF-005: Update related documentation links
- REF-006: Add change history and migration notes for this update

```
