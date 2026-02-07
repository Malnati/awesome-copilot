---
description: "Analise arquivos chatmode ou prompt e recomende modelos de IA ideais com base na complexidade da tarefa, capacidades necessarias e custo-eficiencia"
agent: "agent"
tools:
  - "search/codebase"
  - "fetch"
  - "context7/*"
model: Auto (copilot)
---

# Recomendacao de Modelo de IA para Copilot Chat Modes e Prompts

## Missao

Analise arquivos `.agent.md` ou `.prompt.md` para entender o proposito, a complexidade e as capacidades necessarias, e recomende o(s) modelo(s) de IA mais adequados entre as opcoes disponiveis no GitHub Copilot. Forneca justificativa baseada nas caracteristicas da tarefa, pontos fortes do modelo, custo-eficiencia e trade-offs de performance.

## Escopo e Precondicoes

- **Input**: Caminho para um arquivo `.agent.md` ou `.prompt.md`
- **Modelos Disponiveis**: GPT-4.1, GPT-5, GPT-5 mini, GPT-5 Codex, Claude Sonnet 3.5, Claude Sonnet 4, Claude Sonnet 4.5, Claude Opus 4.1, Gemini 2.5 Pro, Gemini 2.0 Flash, Grok Code Fast 1, o3, o4-mini (com datas de descontinuacao)
- **Auto-Selection de Modelos**: Disponivel no VS Code (Set 2025+) - seleciona entre GPT-4.1, GPT-5 mini, GPT-5, Claude Sonnet 3.5, Claude Sonnet 4.5 (exclui multiplicadores premium > 1)
- **Contexto**: Tiers de assinatura GitHub Copilot (Free: 2K completions + 50 chat/mes com modelos 0x apenas; Pro: ilimitado 0x + 1000 premium/mes; Pro+: ilimitado 0x + 5000 premium/mes)

## Inputs

Obrigatorios:

- `${input:filePath:Path to .agent.md or .prompt.md file}` - Caminho absoluto ou relativo ao workspace do arquivo a analisar

Opcionais:

- `${input:subscriptionTier:Pro}` - Tier de assinatura do Copilot (Free, Pro, Pro+) - padrao Pro
- `${input:priorityFactor:Balanced}` - Prioridade de otimizacao (Speed, Cost, Quality, Balanced) - padrao Balanced

## Workflow

### 1. Fase de Analise do Arquivo

**Ler e Parsear o Arquivo**:

- Leia o arquivo `.agent.md` ou `.prompt.md` alvo
- Extraia o frontmatter (description, mode, tools, model se especificado)
- Analise o corpo para identificar:
  - Complexidade da tarefa (simple/moderate/complex/advanced)
  - Profundidade de raciocinio requerida (basic/intermediate/advanced/expert)
  - Necessidade de geracao de codigo (minimal/moderate/extensive)
  - Requisitos de conversas multi-turn
  - Necessidades de context window (small/medium/large)
  - Capacidades especializadas (image analysis, long-context, real-time data)

**Categorizar Tipo de Tarefa**:

Identifique a categoria principal da tarefa com base na analise de conteudo:

1. **Simple Repetitive Tasks**:

   - Padrao: Formatacao, refatoracao simples, adicionar comentarios/docstrings, CRUD basico
   - Caracteristicas: Logica direta, contexto minimo, execucao rapida preferida
   - Keywords: format, comment, simple, basic, add docstring, rename, move

2. **Code Generation & Implementation**:

   - Padrao: Escrever funcoes/classes, implementar features, endpoints de API, testes
   - Caracteristicas: Complexidade moderada, conhecimento de dominio, codigo idiomatico
   - Keywords: implement, create, generate, write, build, scaffold

3. **Complex Refactoring & Architecture**:

   - Padrao: System design, revisao arquitetural, refatoracao em larga escala, otimizacao de performance
   - Caracteristicas: Raciocinio profundo, multiplos componentes, analise de trade-offs
   - Keywords: architect, refactor, optimize, design, scale, review architecture

4. **Debugging & Problem-Solving**:

   - Padrao: Bug fixing, analise de erros, troubleshooting sistematico, root cause analysis
   - Caracteristicas: Raciocinio passo a passo, contexto de debugging, necessidade de verificacao
   - Keywords: debug, fix, troubleshoot, diagnose, error, investigate

5. **Planning & Research**:

   - Padrao: Planejamento de features, pesquisa, analise de documentacao, criacao de ADR
   - Caracteristicas: Read-only, coleta de contexto, suporte a decisao
   - Keywords: plan, research, analyze, investigate, document, assess

6. **Code Review & Quality Analysis**:

   - Padrao: Analise de seguranca, revisao de performance, validacao de boas praticas, checagem de compliance
   - Caracteristicas: Pensamento critico, reconhecimento de padroes, expertise de dominio
   - Keywords: review, analyze, security, performance, compliance, validate

7. **Specialized Domain Tasks**:

   - Padrao: Django/framework-specific, acessibilidade (WCAG), testing (TDD), design de API
   - Caracteristicas: Conhecimento profundo de dominio, convencoes de framework, compliance com standards
   - Keywords: django, accessibility, wcag, rest, api, testing, tdd

8. **Advanced Reasoning & Multi-Step Workflows**:
   - Padrao: Otimizacao algoritmica, transformacoes complexas de dados, workflows multi-fase
   - Caracteristicas: Raciocinio avancado, pensamento matematico/algoritmico, logica sequencial
   - Keywords: algorithm, optimize, transform, sequential, reasoning, calculate

**Extrair Requisitos de Capacidade**:

Com base em `tools` no frontmatter e nas instrucoes do corpo:

- **Read-only tools** (search, fetch, usages, githubRepo): Menor complexidade, modelos mais rapidos servem
- **Write operations** (edit/editFiles, new): Complexidade moderada, precisao importante
- **Execution tools** (runCommands, runTests, runTasks): Necessidade de validacao, abordagem iterativa
- **Advanced tools** (context7/*, sequential-thinking/*): Raciocinio complexo, modelos premium ajudam
- **Multi-modal** (referencias a image analysis): Requer modelos com visao

### 2. Fase de Avaliacao de Modelos

**Aplicar Criterios de Selecao**:

Para cada modelo disponivel, avalie contra estas dimensoes:

#### Matriz de Capacidades dos Modelos

| Model                   | Multiplier | Speed    | Code Quality | Reasoning | Context | Vision | Best For                                          |
| ----------------------- | ---------- | -------- | ------------ | --------- | ------- | ------ | ------------------------------------------------- |
| GPT-4.1                 | 0x         | Fast     | Good         | Good      | 128K    | ✅     | Balanced general tasks, included in all plans     |
| GPT-5 mini              | 0x         | Fastest  | Good         | Basic     | 128K    | ❌     | Simple tasks, quick responses, cost-effective     |
| GPT-5                   | 1x         | Moderate | Excellent    | Advanced  | 128K    | ✅     | Complex code, advanced reasoning, multi-turn chat |
| GPT-5 Codex             | 1x         | Fast     | Excellent    | Good      | 128K    | ❌     | Code optimization, refactoring, algorithmic tasks |
| Claude Sonnet 3.5       | 1x         | Moderate | Excellent    | Excellent | 200K    | ✅     | Code generation, long context, balanced reasoning |
| Claude Sonnet 4         | 1x         | Moderate | Excellent    | Advanced  | 200K    | ❌     | Complex code, robust reasoning, enterprise tasks  |
| Claude Sonnet 4.5       | 1x         | Moderate | Excellent    | Expert    | 200K    | ✅     | Advanced code, architecture, design patterns      |
| Claude Opus 4.1         | 10x        | Slow     | Outstanding  | Expert    | 1M      | ✅     | Large codebases, architectural review, research   |
| Gemini 2.5 Pro          | 1x         | Moderate | Excellent    | Advanced  | 2M      | ✅     | Very long context, multi-modal, real-time data    |
| Gemini 2.0 Flash (dep.) | 0.25x      | Fastest  | Good         | Good      | 1M      | ❌     | Fast responses, cost-effective (deprecated)       |
| Grok Code Fast 1        | 0.25x      | Fastest  | Good         | Basic     | 128K    | ❌     | Speed-critical simple tasks, preview (free)       |
| o3 (deprecated)         | 1x         | Slow     | Good         | Expert    | 128K    | ❌     | Advanced reasoning, algorithmic optimization      |
| o4-mini (deprecated)    | 0.33x      | Fast     | Good         | Good      | 128K    | ❌     | Reasoning at lower cost (deprecated)              |

#### Arvore de Decisao

```
START
  │
  ├─ Task Complexity?
  │   ├─ Simple/Repetitive → GPT-5 mini, Grok Code Fast 1, GPT-4.1
  │   ├─ Moderate → GPT-4.1, Claude Sonnet 4, GPT-5
  │   └─ Complex/Advanced → Claude Sonnet 4.5, GPT-5, Gemini 2.5 Pro, Claude Opus 4.1
  │
  ├─ Reasoning Depth?
  │   ├─ Basic → GPT-5 mini, Grok Code Fast 1
  │   ├─ Intermediate → GPT-4.1, Claude Sonnet 4
  │   ├─ Advanced → GPT-5, Claude Sonnet 4.5
  │   └─ Expert → Claude Opus 4.1, o3 (deprecated)
  │
  ├─ Code-Specific?
  │   ├─ Yes → GPT-5 Codex, Claude Sonnet 4.5, GPT-5
  │   └─ No → GPT-5, Claude Sonnet 4
  │
  ├─ Context Size?
  │   ├─ Small (<50K tokens) → Any model
  │   ├─ Medium (50-200K) → Claude models, GPT-5, Gemini
  │   ├─ Large (200K-1M) → Gemini 2.5 Pro, Claude Opus 4.1
  │   └─ Very Large (>1M) → Gemini 2.5 Pro (2M), Claude Opus 4.1 (1M)
  │
  ├─ Vision Required?
  │   ├─ Yes → GPT-4.1, GPT-5, Claude Sonnet 3.5/4.5, Gemini 2.5 Pro, Claude Opus 4.1
  │   └─ No → All models
  │
  ├─ Cost Sensitivity? (based on subscriptionTier)
  │   ├─ Free Tier → 0x models only: GPT-4.1, GPT-5 mini, Grok Code Fast 1
  │   ├─ Pro (1000 premium/month) → Prioritize 0x, use 1x judiciously, avoid 10x
  │   └─ Pro+ (5000 premium/month) → 1x freely, 10x for critical tasks
  │
  └─ Priority Factor?
      ├─ Speed → GPT-5 mini, Grok Code Fast 1, Gemini 2.0 Flash
      ├─ Cost → 0x models (GPT-4.1, GPT-5 mini) or lower multipliers (0.25x, 0.33x)
      ├─ Quality → Claude Sonnet 4.5, GPT-5, Claude Opus 4.1
      └─ Balanced → GPT-4.1, Claude Sonnet 4, GPT-5
```

### 3. Fase de Geracao da Recomendacao

**Recomendacao Primaria**:

- Identifique o melhor modelo com base na analise da tarefa e na arvore de decisao
- Forneca uma justificativa especifica vinculada ao conteudo do arquivo
- Explique implicacoes de custo do multiplicador para o tier do usuario

**Recomendacoes Alternativas**:

- Sugira 1-2 modelos alternativos com explicacoes de trade-offs
- Inclua cenarios em que alternativas podem ser preferiveis
- Considere overrides de priority factor (speed vs. quality vs. cost)

**Orientacao de Auto-Selection**:

- Avalie se a tarefa e adequada para auto-selecao (exclui modelos premium > 1x)
- Explique quando a selecao manual e benefica vs. deixar o Copilot escolher
- Observe limitacoes da auto-selecao para a tarefa especifica

**Avisos de Descontinuacao**:

- Sinalize se o arquivo especifica um modelo deprecated (o3, o4-mini, Claude Sonnet 3.7, Gemini 2.0 Flash)
- Forneca caminho de migracao para a recomendacao de substituicao
- Inclua timeline para descontinuacao (por exemplo, "o3 deprecating 2025-10-23")

**Consideracoes por Subscription Tier**:

- **Free Tier**: Recomende apenas modelos com multiplicador 0x (GPT-4.1, GPT-5 mini, Grok Code Fast 1)
- **Pro Tier**: Balanceie entre 0x (ilimitado) e 1x (1000/mes)
- **Pro+ Tier**: Mais liberdade com modelos 1x (5000/mes), justifique uso 10x para casos excepcionais

### 4. Recomendacoes de Integracao

**Orientacao para Atualizacao de Frontmatter**:

Se o arquivo nao especifica `model`:

```markdown
## Recommendation: Add Model Specification

Current frontmatter:
\`\`\`yaml

---

description: "..."
tools: [...]

---

\`\`\`

Recommended frontmatter:
\`\`\`yaml

---

description: "..."
model: "[Recommended Model Name]"
tools: [...]

---

\`\`\`

Rationale: [Explanation of why this model is optimal for this task]
```

Se o arquivo ja especifica um modelo:

```markdown
## Current Model Assessment

Specified model: `[Current Model]` (Multiplier: [X]x)

Recommendation: [Keep current model | Consider switching to [Recommended Model]]

Rationale: [Explanation]
```

**Checagem de Alinhamento de Tools**:

Verifique se as capacidades do modelo alinham com as tools especificadas:

- Se tools incluem `context7/*` ou `sequential-thinking/*`: Recomende modelos com raciocinio avancado (Claude Sonnet 4.5, GPT-5, Claude Opus 4.1)
- Se tools incluem referencias a visao: Garanta suporte a imagens (sinalize se GPT-5 Codex, Claude Sonnet 4 ou mini models selecionados)
- Se tools sao read-only (search, fetch): Sugira modelos custo-efetivos (GPT-5 mini, Grok Code Fast 1)

### 5. Integracao com Context7 para Informacoes Atualizadas

**Use Context7 para Documentacao de Modelos**:

Quando houver duvida sobre capacidades atuais dos modelos, use Context7 para buscar informacoes mais recentes:

```markdown
**Verification with Context7**:

Using `context7/get-library-docs` with library ID `/websites/github_en_copilot`:

- Query topic: "model capabilities [specific capability question]"
- Retrieve current model features, multipliers, deprecation status
- Cross-reference against analyzed file requirements
```

**Exemplo de Uso do Context7**:

```
If unsure whether Claude Sonnet 4.5 supports image analysis:
→ Use context7 with topic "Claude Sonnet 4.5 vision image capabilities"
→ Confirm feature support before recommending for multi-modal tasks
```

## Expectativas de Saida

### Estrutura do Relatorio

Gere um relatorio markdown estruturado com as seguintes secoes:

```markdown
# AI Model Recommendation Report

**File Analyzed**: `[file path]`
**File Type**: [chatmode | prompt]
**Analysis Date**: [YYYY-MM-DD]
**Subscription Tier**: [Free | Pro | Pro+]

---

## File Summary

**Description**: [from frontmatter]
**Mode**: [ask | edit | agent]
**Tools**: [tool list]
**Current Model**: [specified model or "Not specified"]

## Task Analysis

### Task Complexity

- **Level**: [Simple | Moderate | Complex | Advanced]
- **Reasoning Depth**: [Basic | Intermediate | Advanced | Expert]
- **Context Requirements**: [Small | Medium | Large | Very Large]
- **Code Generation**: [Minimal | Moderate | Extensive]
- **Multi-Modal**: [Yes | No]

### Task Category

[Primary category from 8 categories listed in Workflow Phase 1]

### Key Characteristics

- Characteristic 1: [explanation]
- Characteristic 2: [explanation]
- Characteristic 3: [explanation]

## Model Recommendation

### 🏆 Primary Recommendation: [Model Name]

**Multiplier**: [X]x ([cost implications for subscription tier])
**Strengths**:

- Strength 1: [specific to task]
- Strength 2: [specific to task]
- Strength 3: [specific to task]

**Rationale**:
[Detailed explanation connecting task characteristics to model capabilities]

**Cost Impact** (for [Subscription Tier]):

- Per request multiplier: [X]x
- Estimated usage: [rough estimate based on task frequency]
- [Additional cost context]

### 🔄 Alternative Options

#### Option 1: [Model Name]

- **Multiplier**: [X]x
- **When to Use**: [specific scenarios]
- **Trade-offs**: [compared to primary recommendation]

#### Option 2: [Model Name]

- **Multiplier**: [X]x
- **When to Use**: [specific scenarios]
- **Trade-offs**: [compared to primary recommendation]

### 📊 Model Comparison for This Task

| Criterion        | [Primary Model] | [Alternative 1] | [Alternative 2] |
| ---------------- | --------------- | --------------- | --------------- |
| Task Fit         | ⭐⭐⭐⭐⭐      | ⭐⭐⭐⭐        | ⭐⭐⭐          |
| Code Quality     | [rating]        | [rating]        | [rating]        |
| Reasoning        | [rating]        | [rating]        | [rating]        |
| Speed            | [rating]        | [rating]        | [rating]        |
| Cost Efficiency  | [rating]        | [rating]        | [rating]        |
| Context Capacity | [capacity]      | [capacity]      | [capacity]      |
| Vision Support   | [Yes/No]        | [Yes/No]        | [Yes/No]        |

## Auto Model Selection Assessment

**Suitability**: [Recommended | Not Recommended | Situational]

[Explanation of whether auto-selection is appropriate for this task]

**Rationale**:

- [Reason 1]
- [Reason 2]

**Manual Override Scenarios**:

- [Scenario where user should manually select model]
- [Scenario where user should manually select model]

## Implementation Guidance

### Frontmatter Update

[Provide specific code block showing recommended frontmatter change]
```
