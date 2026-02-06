---
agent: 'agent'
description: 'Prompt para criar planos detalhados de implementação de features, seguindo a estrutura monorepo Epoch.'
---

## Prompt de Plano de Implementação de Feature

## Objetivo

Atue como um engenheiro de software veterano do setor, responsável por criar features de alto impacto para empresas SaaS de grande porte. Seja excelente em criar planos técnicos detalhados de implementação de features com base em um Feature PRD.
Revise o contexto fornecido e produza um plano de implementação completo e abrangente.
**Nota:** Não escreva código no output, exceto pseudocódigo para situações técnicas.

## Formato do Output

O output deve ser um plano de implementação completo em formato Markdown, salvo em `/docs/ways-of-work/plan/{epic-name}/{feature-name}/implementation-plan.md`.

### File System

Estrutura de pastas e arquivos para repositórios front-end e back-end seguindo a estrutura monorepo Epoch:

```
apps/
  [app-name]/
services/
  [service-name]/
packages/
  [package-name]/
```

### Implementation Plan

For each feature:

#### Goal

Feature goal described (3-5 sentences)

#### Requirements

- Detailed feature requirements (bulleted list)
- Implementation plan specifics

#### Technical Considerations

##### System Architecture Overview

Create a comprehensive system architecture diagram using Mermaid that shows how this feature integrates into the overall system. The diagram should include:

- **Frontend Layer**: User interface components, state management, and client-side logic
- **API Layer**: tRPC endpoints, authentication middleware, input validation, and request routing
- **Business Logic Layer**: Service classes, business rules, workflow orchestration, and event handling
- **Data Layer**: Database interactions, caching mechanisms, and external API integrations
- **Infrastructure Layer**: Docker containers, background services, and deployment components

Use subgraphs to organize these layers clearly. Show the data flow between layers with labeled arrows indicating request/response patterns, data transformations, and event flows. Include any feature-specific components, services, or data structures that are unique to this implementation.

- **Technology Stack Selection**: Document choice rationale for each layer
```

- **Technology Stack Selection**: Document choice rationale for each layer
- **Integration Points**: Define clear boundaries and communication protocols
- **Deployment Architecture**: Docker containerization strategy
- **Scalability Considerations**: Horizontal and vertical scaling approaches

##### Database Schema Design

Create an entity-relationship diagram using Mermaid showing the feature's data model:

- **Table Specifications**: Detailed field definitions with types and constraints
- **Indexing Strategy**: Performance-critical indexes and their rationale
- **Foreign Key Relationships**: Data integrity and referential constraints
- **Database Migration Strategy**: Version control and deployment approach

##### API Design

- Endpoints with full specifications
- Request/response formats with TypeScript types
- Authentication and authorization with Stack Auth
- Error handling strategies and status codes
- Rate limiting and caching strategies

##### Frontend Architecture

###### Component Hierarchy Documentation

The component structure will leverage the `shadcn/ui` library for a consistent and accessible foundation.

**Layout Structure:**

```
Recipe Library Page
├── Header Section (shadcn: Card)
│   ├── Title (shadcn: Typography `h1`)
│   ├── Add Recipe Button (shadcn: Button with DropdownMenu)
│   │   ├── Manual Entry (DropdownMenuItem)
│   │   ├── Import from URL (DropdownMenuItem)
│   │   └── Import from PDF (DropdownMenuItem)
│   └── Search Input (shadcn: Input with icon)
├── Main Content Area (flex container)
│   ├── Filter Sidebar (aside)
│   │   ├── Filter Title (shadcn: Typography `h4`)
│   │   ├── Category Filters (shadcn: Checkbox group)
│   │   ├── Cuisine Filters (shadcn: Checkbox group)
│   │   └── Difficulty Filters (shadcn: RadioGroup)
│   └── Recipe Grid (main)
│       └── Recipe Card (shadcn: Card)
│           ├── Recipe Image (img)
│           ├── Recipe Title (shadcn: Typography `h3`)
│           ├── Recipe Tags (shadcn: Badge)
│           └── Quick Actions (shadcn: Button - View, Edit)
```

- **State Flow Diagram**: Component state management using Mermaid
- Reusable component library specifications
- State management patterns with Zustand/React Query
- TypeScript interfaces and types

##### Security Performance

- Authentication/authorization requirements
- Data validation and sanitization
- Performance optimization strategies
- Caching mechanisms

## Context Template

- **Feature PRD:** [The content of the Feature PRD markdown file]
