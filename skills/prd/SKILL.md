---
name: prd
description: 'Gere PRDs (Product Requirements Documents) de alta qualidade para sistemas de software e features com IA. Inclui resumos executivos, user stories, especificacoes tecnicas e analise de riscos.'
license: MIT
---

# Documento de Requisitos de Produto (PRD)

## Visao Geral

Desenhe PRDs abrangentes e prontos para producao que fazem a ponte entre a visao de negocio e a execucao tecnica. Esta skill funciona para sistemas de software modernos, garantindo que requisitos sejam claramente definidos.

## Quando Usar

Use esta skill quando:

- Iniciar um novo ciclo de desenvolvimento de produto ou feature
- Traduzir uma ideia vaga em especificacao tecnica concreta
- Definir requisitos para features com IA
- Stakeholders precisam de uma "source of truth" unificada para o escopo do projeto
- Usuario pede para "escrever um PRD", "documentar requisitos" ou "planejar uma feature"

---

## Workflow Operacional

### Fase 1: Discovery (A Entrevista)

Antes de escrever uma linha do PRD, voce **DEVE** interrogar o usuario para preencher lacunas de conhecimento. Nao assuma contexto.

**Pergunte sobre:**

- **O Problema Central**: Por que estamos construindo isso agora?
- **Metricas de Sucesso**: Como sabemos que funcionou?
- **Restricoes**: Budget, stack tecnica ou deadline?

### Fase 2: Analise e Escopo

Sintetize a entrada do usuario. Identifique dependencias e complexidades ocultas.

- Mapeie o **User Flow**.
- Defina **Non-Goals** para proteger o timeline.

### Fase 3: Redacao Tecnica

Gere o documento usando o **Schema Estrito de PRD** abaixo.

---

## Padroes de Qualidade do PRD

### Qualidade de Requisitos

Use criterios concretos e mensuraveis. Evite "rapido", "facil" ou "intuitivo".

```diff
# Vague (BAD)
- The search should be fast and return relevant results.
- The UI must look modern and be easy to use.

# Concrete (GOOD)
+ The search must return results within 200ms for a 10k record dataset.
+ The search algorithm must achieve >= 85% Precision@10 in benchmark evals.
+ The UI must follow the 'Vercel/Next.js' design system and achieve 100% Lighthouse Accessibility score.
```

---

## Schema Estrito de PRD

Voce **DEVE** seguir esta estrutura exata para a saida:

### 1. Resumo Executivo

- **Problem Statement**: 1-2 frases sobre o problema.
- **Proposed Solution**: 1-2 frases sobre a solucao.
- **Success Criteria**: 3-5 KPIs mensuraveis.

### 2. User Experience & Functionality

- **User Personas**: Para quem e isso?
- **User Stories**: `As a [user], I want to [action] so that [benefit].`
- **Acceptance Criteria**: Lista de definicoes de "Done" para cada story.
- **Non-Goals**: O que NAO vamos construir?

### 3. Requisitos do Sistema de IA (Se aplicavel)

- **Requisitos de Tools**: Quais tools e APIs sao necessarias?
- **Evaluation Strategy**: Como medir qualidade e acuracia da saida.

### 4. Technical Specifications

- **Visao Geral de Arquitetura**: Fluxo de dados e interacao de componentes.
- **Integration Points**: APIs, DBs e Auth.
- **Seguranca e Privacidade**: Tratamento de dados e compliance.

### 5. Risks & Roadmap

- **Phased Rollout**: MVP -> v1.1 -> v2.0.
- **Technical Risks**: Latencia, custo ou falhas de dependencias.

---

## Diretrizes de Implementacao

### DO (Sempre)

- **Definir Testes**: Para sistemas de IA, especifique como testar e validar qualidade da saida.
- **Iterar**: Apresente um draft e peca feedback sobre secoes especificas.

### DON'T (Evitar)

- **Pular Discovery**: Nunca escreva um PRD sem fazer pelo menos 2 perguntas de esclarecimento.
- **Alucinar Restricoes**: Se o usuario nao especificou uma stack, pergunte ou marque como `TBD`.

---

## Exemplo: Intelligent Search System

### 1. Resumo Executivo

**Problem**: Users struggle to find specific documentation snippets in massive repositories.
**Solution**: An intelligent search system that provides direct answers with source citations.
**Success**:

- Reduce search time by 50%.
- Citation accuracy >= 95%.

### 2. User Stories

- **Story**: As a developer, I want to ask natural language questions so I don't have to guess keywords.
- **AC**:
  - Supports multi-turn clarification.
  - Returns code blocks with "Copy" button.

### 3. AI System Architecture

- **Tools Required**: `codesearch`, `grep`, `webfetch`.

### 4. Evaluation

- **Benchmark**: Test with 50 common developer questions.
- **Pass Rate**: 90% must match expected citations.