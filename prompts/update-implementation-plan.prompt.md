---
agent: 'agent'
description: 'Atualize um arquivo de plano de implementacao existente com novos ou atualizados requisitos para fornecer novas funcionalidades, refatorar codigo existente ou atualizar pacotes, design, arquitetura ou infraestrutura.'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'extensions', 'web/fetch', 'githubRepo', 'openSimpleBrowser', 'problems', 'runTasks', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# Atualizar Plano de Implementacao

## Diretriz Primaria

Voce e um agente de IA encarregado de atualizar o arquivo de plano de implementacao `${file}` com base em requisitos novos ou atualizados. Sua saida deve ser legivel por maquina, deterministica e estruturada para execucao autonoma por outros sistemas de IA ou humanos.

## Contexto de Execucao

Este prompt foi projetado para comunicacao IA-para-IA e processamento automatizado. Todas as instrucoes devem ser interpretadas literalmente e executadas de forma sistematica, sem interpretacao humana ou esclarecimentos.

## Requisitos Centrais

- Gerar planos de implementacao totalmente executaveis por agentes de IA ou humanos
- Usar linguagem deterministica com zero ambiguidade
- Estruturar todo o conteudo para parsing e execucao automatizados
- Garantir total auto-contencao sem dependencias externas para entendimento

## Requisitos de Estrutura do Plano

Planos devem consistir de fases discretas e atomicas contendo tarefas executaveis. Cada fase deve ser processavel de forma independente por agentes de IA ou humanos, sem dependencias entre fases, a menos que explicitamente declaradas.

## Arquitetura de Fases

- Cada fase deve ter criterios de conclusao mensuraveis
- Tarefas dentro das fases devem ser executaveis em paralelo, a menos que dependencias sejam especificadas
- Todas as descricoes de tarefas devem incluir caminhos de arquivos especificos, nomes de funcoes e detalhes exatos de implementacao
- Nenhuma tarefa deve exigir interpretacao humana ou tomada de decisao

## Padroes de Implementacao Otimizados para IA

- Use linguagem explicita e sem ambiguidades, sem necessidade de interpretacao
- Estruture todo o conteudo como formatos parseaveis por maquina (tabelas, listas, dados estruturados)
- Inclua caminhos de arquivos especificos, numeros de linha e referencias exatas de codigo quando aplicavel
- Defina todas as variaveis, constantes e valores de configuracao explicitamente
- Forneca contexto completo em cada descricao de tarefa
- Use prefixos padronizados para todos os identificadores (REQ-, TASK-, etc.)
- Inclua criterios de validacao que possam ser verificados automaticamente

## Especificacoes do Arquivo de Saida

- Salve arquivos de plano de implementacao no diretorio `/plan/`
- Use convencao de nome: `[purpose]-[component]-[version].md`
- Prefixos de objetivo: `upgrade|refactor|feature|data|infrastructure|process|architecture|design`
- Exemplo: `upgrade-system-command-4.md`, `feature-auth-module-1.md`
- O arquivo deve ser Markdown valido com estrutura correta de front matter

## Estrutura de Template Obrigatoria

Todos os planos de implementacao devem aderir estritamente ao template abaixo. Cada secao e obrigatoria e deve ser preenchida com conteudo especifico e acionavel. Agentes de IA devem validar conformidade do template antes da execucao.

## Regras de Validacao do Template

- Todos os campos do front matter devem estar presentes e formatados corretamente
- Todos os cabecalhos de secao devem corresponder exatamente (sensivel a maiusculas/minusculas)
- Todos os prefixos de identificadores devem seguir o formato especificado
- Tabelas devem incluir todas as colunas obrigatorias
- Nenhum texto placeholder pode permanecer na saida final

## Status

O status do plano de implementacao deve ser claramente definido no front matter e refletir o estado atual do plano. O status pode ser um dos seguintes (status_color entre colchetes): `Completed` (bright green badge), `In progress` (yellow badge), `Planned` (blue badge), `Deprecated` (red badge) ou `On Hold` (orange badge). Ele tambem deve ser exibido como um badge na secao de introducao.

```md
---
goal: [Concise Title Describing the Package Implementation Plan's Goal]
version: [Optional: e.g., 1.0, Date]
date_created: [YYYY-MM-DD]
last_updated: [Optional: YYYY-MM-DD]
owner: [Optional: Team/Individual responsible for this spec]
status: 'Completed'|'In progress'|'Planned'|'Deprecated'|'On Hold'
tags: [Optional: List of relevant tags or categories, e.g., `feature`, `upgrade`, `chore`, `architecture`, `migration`, `bug` etc]
---

# Introduction

![Status: <status>](https://img.shields.io/badge/status-<status>-<status_color>)

[A short concise introduction to the plan and the goal it is intended to achieve.]

## 1. Requirements & Constraints

[Explicitly list all requirements & constraints that affect the plan and constrain how it is implemented. Use bullet points or tables for clarity.]

- **REQ-001**: Requirement 1
- **SEC-001**: Security Requirement 1
- **[3 LETTERS]-001**: Other Requirement 1
- **CON-001**: Constraint 1
- **GUD-001**: Guideline 1
- **PAT-001**: Pattern to follow 1

## 2. Implementation Steps

### Implementation Phase 1

- GOAL-001: [Describe the goal of this phase, e.g., "Implement feature X", "Refactor module Y", etc.]

| Task | Description | Completed | Date |
|------|-------------|-----------|------|
| TASK-001 | Description of task 1 | ✅ | 2025-04-25 |
| TASK-002 | Description of task 2 | |  |
| TASK-003 | Description of task 3 | |  |

### Implementation Phase 2

- GOAL-002: [Describe the goal of this phase, e.g., "Implement feature X", "Refactor module Y", etc.]

| Task | Description | Completed | Date |
|------|-------------|-----------|------|
| TASK-004 | Description of task 4 | |  |
| TASK-005 | Description of task 5 | |  |
| TASK-006 | Description of task 6 | |  |

## 3. Alternatives

[A bullet point list of any alternative approaches that were considered and why they were not chosen. This helps to provide context and rationale for the chosen approach.]

- **ALT-001**: Alternative approach 1
- **ALT-002**: Alternative approach 2

## 4. Dependencies

[List any dependencies that need to be addressed, such as libraries, frameworks, or other components that the plan relies on.]

- **DEP-001**: Dependency 1
- **DEP-002**: Dependency 2

## 5. Files

[List the files that will be affected by the feature or refactoring task.]

- **FILE-001**: Description of file 1
- **FILE-002**: Description of file 2

## 6. Testing

[List the tests that need to be implemented to verify the feature or refactoring task.]

- **TEST-001**: Description of test 1
- **TEST-002**: Description of test 2

## 7. Risks & Assumptions

[List any risks or assumptions related to the implementation of the plan.]

- **RISK-001**: Risk 1
- **ASSUMPTION-001**: Assumption 1

## 8. Related Specifications / Further Reading

[Link to related spec 1]
[Link to relevant external documentation]
```
