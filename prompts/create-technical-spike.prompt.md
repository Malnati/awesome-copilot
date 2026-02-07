---
agent: 'agent'
description: 'Crie documentos de technical spike com timebox para pesquisar e resolver decisoes criticas de desenvolvimento antes da implementacao.'
tools: ['runCommands', 'runTasks', 'edit', 'search', 'extensions', 'usages', 'vscodeAPI', 'think', 'problems', 'changes', 'testFailure', 'openSimpleBrowser', 'web/fetch', 'githubRepo', 'todos', 'Microsoft Docs', 'search']
---

# Criar Documento de Technical Spike

Crie documentos de technical spike com timebox para pesquisar questoes criticas que precisam ser respondidas antes do desenvolvimento. Cada spike foca em uma decisao tecnica especifica com entregaveis e timeline claros.

## Estrutura do Documento

Crie arquivos individuais no diretorio `${input:FolderPath|docs/spikes}`. Nomeie cada arquivo com o pattern: `[category]-[short-description]-spike.md` (ex.: `api-copilot-integration-spike.md`, `performance-realtime-audio-spike.md`).

```md
---
title: "${input:SpikeTitle}"
category: "${input:Category|Technical}"
status: "🔴 Not Started"
priority: "${input:Priority|High}"
timebox: "${input:Timebox|1 week}"
created: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
owner: "${input:Owner}"
tags: ["technical-spike", "${input:Category|technical}", "research"]
---

# ${input:SpikeTitle}

## Summary

**Spike Objective:** [Clear, specific question or decision that needs resolution]

**Why This Matters:** [Impact on development/architecture decisions]

**Timebox:** [How much time allocated to this spike]

**Decision Deadline:** [When this must be resolved to avoid blocking development]

## Research Question(s)

**Primary Question:** [Main technical question that needs answering]

**Secondary Questions:**

- [Related question 1]
- [Related question 2]
- [Related question 3]

## Investigation Plan

### Research Tasks

- [ ] [Specific research task 1]
- [ ] [Specific research task 2]
- [ ] [Specific research task 3]
- [ ] [Create proof of concept/prototype]
- [ ] [Document findings and recommendations]

### Success Criteria

**This spike is complete when:**

- [ ] [Specific criteria 1]
- [ ] [Specific criteria 2]
- [ ] [Clear recommendation documented]
- [ ] [Proof of concept completed (if applicable)]

## Technical Context

**Related Components:** [List system components affected by this decision]

**Dependencies:** [What other spikes or decisions depend on resolving this]

**Constraints:** [Known limitations or requirements that affect the solution]

## Research Findings

### Investigation Results

[Document research findings, test results, and evidence gathered]

### Prototype/Testing Notes

[Results from any prototypes, spikes, or technical experiments]

### External Resources

- [Link to relevant documentation]
- [Link to API references]
- [Link to community discussions]
- [Link to examples/tutorials]

## Decision

### Recommendation

[Clear recommendation based on research findings]

### Rationale

[Why this approach was chosen over alternatives]

### Implementation Notes

[Key considerations for implementation]

### Follow-up Actions

- [ ] [Action item 1]
- [ ] [Action item 2]
- [ ] [Update architecture documents]
- [ ] [Create implementation tasks]

## Status History

| Date   | Status         | Notes                      |
| ------ | -------------- | -------------------------- |
| [Date] | 🔴 Not Started | Spike created and scoped   |
| [Date] | 🟡 In Progress | Research commenced         |
| [Date] | 🟢 Complete    | [Resolution summary]       |

---

_Last updated: [Date] by [Name]_
```

## Categorias para Technical Spikes

### API Integration

- Capacidades e limitacoes de APIs de terceiros
- Patterns de integracao e autenticacao
- Rate limits e caracteristicas de performance

### Architecture & Design

- Decisoes de arquitetura do sistema
- Aplicabilidade de design patterns
- Modelos de interacao entre componentes

### Performance & Scalability

- Requisitos e restricoes de performance
- Gargalos de escalabilidade e solucoes
- Padroes de uso de recursos

### Platform & Infrastructure

- Capacidades e limitacoes da plataforma
- Requisitos de infraestrutura
- Consideracoes de deploy e hosting

### Security & Compliance

- Requisitos e implementacoes de seguranca
- Restricoes de compliance
- Abordagens de autenticacao e autorizacao

### User Experience

- Padroes de interacao do usuario
- Requisitos de acessibilidade
- Decisoes de design de interface

## Convencoes de Nome de Arquivo

Use nomes descritivos em kebab-case que indiquem a categoria e o desconhecido especifico:

**API/Integration Examples:**

- `api-copilot-chat-integration-spike.md`
- `api-azure-speech-realtime-spike.md`
- `api-vscode-extension-capabilities-spike.md`

**Performance Examples:**

- `performance-audio-processing-latency-spike.md`
- `performance-extension-host-limitations-spike.md`
- `performance-webrtc-reliability-spike.md`

**Architecture Examples:**

- `architecture-voice-pipeline-design-spike.md`
- `architecture-state-management-spike.md`
- `architecture-error-handling-strategy-spike.md`

## Best Practices para Agentes de IA

1. **Uma Questao por Spike:** Cada documento foca em uma decisao tecnica ou questao de pesquisa

2. **Pesquisa com Timebox:** Defina limites de tempo e entregaveis especificos para cada spike

3. **Decisoes Baseadas em Evidencia:** Exija evidencias concretas (testes, prototipos, documentacao) antes de marcar como completo

4. **Recomendacoes Claras:** Documente recomendacoes e racional

5. **Dependency Tracking:** Identifique como spikes se relacionam e impactam decisoes do projeto

6. **Foco em Resultado:** Todo spike deve resultar em uma decisao ou recomendacao acionavel
