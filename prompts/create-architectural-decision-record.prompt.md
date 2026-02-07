---
agent: 'agent'
description: 'Crie um Architectural Decision Record (ADR) para documentacao de decisoes otimizada para IA.'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'extensions', 'web/fetch', 'githubRepo', 'openSimpleBrowser', 'problems', 'runTasks', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# Criar Architectural Decision Record

Crie um ADR para `${input:DecisionTitle}` usando formatacao estruturada otimizada para consumo por IA e leitura humana.

## Inputs

- **Context**: `${input:Context}`
- **Decision**: `${input:Decision}`
- **Alternatives**: `${input:Alternatives}`
- **Stakeholders**: `${input:Stakeholders}`

## Input Validation
Se algum dos inputs obrigatorios nao for fornecido ou nao puder ser determinado pelo historico da conversa, solicite ao usuario as informacoes faltantes antes de prosseguir com a geracao do ADR.

## Requisitos

- Use linguagem precisa e sem ambiguidades
- Siga formato padronizado de ADR com front matter
- Inclua consequencias positivas e negativas
- Documente alternativas com justificativa de rejeicao
- Estruture para parsing por maquina e referencia humana
- Use coded bullet points (3-4 letter codes + 3-digit numbers) para secoes com multiplos itens

O ADR deve ser salvo em `/docs/adr/` usando a convencao: `adr-NNNN-[title-slug].md`, onde NNNN e o proximo numero sequencial de 4 digitos (ex.: `adr-0001-database-selection.md`).

## Estrutura de Documentacao Obrigatoria

O arquivo deve seguir o template abaixo, garantindo que todas as secoes sejam preenchidas. O front matter deve ser estruturado corretamente conforme o exemplo:

```md
---
title: "ADR-NNNN: [Decision Title]"
status: "Proposed"
date: "YYYY-MM-DD"
authors: "[Stakeholder Names/Roles]"
tags: ["architecture", "decision"]
supersedes: ""
superseded_by: ""
---

# ADR-NNNN: [Decision Title]

## Status

**Proposed** | Accepted | Rejected | Superseded | Deprecated

## Context

[Problem statement, technical constraints, business requirements, and environmental factors requiring this decision.]

## Decision

[Chosen solution with clear rationale for selection.]

## Consequences

### Positive

- **POS-001**: [Beneficial outcomes and advantages]
- **POS-002**: [Performance, maintainability, scalability improvements]
- **POS-003**: [Alignment with architectural principles]

### Negative

- **NEG-001**: [Trade-offs, limitations, drawbacks]
- **NEG-002**: [Technical debt or complexity introduced]
- **NEG-003**: [Risks and future challenges]

## Alternatives Considered

### [Alternative 1 Name]

- **ALT-001**: **Description**: [Brief technical description]
- **ALT-002**: **Rejection Reason**: [Why this option was not selected]

### [Alternative 2 Name]

- **ALT-003**: **Description**: [Brief technical description]
- **ALT-004**: **Rejection Reason**: [Why this option was not selected]

## Implementation Notes

- **IMP-001**: [Key implementation considerations]
- **IMP-002**: [Migration or rollout strategy if applicable]
- **IMP-003**: [Monitoring and success criteria]

## References

- **REF-001**: [Related ADRs]
- **REF-002**: [External documentation]
- **REF-003**: [Standards or frameworks referenced]
```
