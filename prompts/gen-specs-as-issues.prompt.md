---
description: 'Este workflow orienta uma abordagem sistematica para identificar features faltantes, prioriza-las e criar especificacoes detalhadas para implementacao.'
agent: 'agent'
---

# Product Manager Assistant: Identificacao de Features e Especificacao

Este workflow orienta uma abordagem sistematica para identificar features faltantes, prioriza-las e criar especificacoes detalhadas para implementacao.

## 1. Fase de Entendimento do Projeto

- Revise a estrutura do projeto para entender sua organizacao
- Leia README.md e outros arquivos de documentacao para entender a funcionalidade central
- Identifique o status de implementacao examinando:
  - Pontos de entrada principais (CLI, API, UI, etc.)
  - Modulos centrais e suas funcionalidades
  - Testes para entender o comportamento esperado
  - Implementacoes placeholder

**Perguntas Norteadoras:**
- Qual e o proposito principal do projeto?
- Quais problemas do usuario ele resolve?
- Quais patterns existem na implementacao atual?
- Quais features sao mencionadas na documentacao mas nao estao totalmente implementadas?

## 2. Fase de Gap Analysis

- Compare capacidades documentadas APENAS contra a implementacao real
- Identifique codigo "placeholder" sem funcionalidade real
- Procure features mencionadas na documentacao mas sem implementacao robusta
- Considere a jornada do usuario e identifique passos quebrados ou faltantes
- Foque primeiro na funcionalidade core (nao em nice-to-have)

**Criacao de Output:**
- Crie uma lista de possiveis features faltantes (5-7 itens)
- Para cada feature, registre:
  - Status atual de implementacao
  - Referencias na documentacao
  - Impacto na experiencia do usuario se estiver faltando

## 3. Fase de Priorizacao

- Aplique uma pontuacao para cada gap identificado:

**Matriz de Pontuacao (1-5):**
- Impacto no Usuario: Quantos usuarios se beneficiam?
- Alinhamento Estrategico: Se encaixa na missao core?
- Viabilidade de Implementacao: Complexidade tecnica?
- Recursos Necessarios: Esforco de desenvolvimento?
- Nivel de Risco: Impactos negativos potenciais?

**Priority = (User Impact × Strategic Alignment) / (Implementation Effort × Risk Level)**

**Criacao de Output:**
- Apresente as 3 features faltantes de maior prioridade com base na pontuacao
- Para cada uma, forneca:
  - Nome da feature
  - Status atual
  - Impacto se nao for implementada
  - Dependencias com outras features

## 4. Fase de Desenvolvimento de Especificacao

- Para cada feature priorizada, desenvolva uma especificacao detalhada mas pratica:
  - Comece com a abordagem filosofica: simplicidade > complexidade
  - Foque primeiro no MVP
  - Considere a experiencia do dev
  - Mantenha a especificacao amigavel para implementacao

**Para Cada Especificacao de Feature:**
1. **Overview & Scope**
   - Que problema resolve?
   - O que esta incluido e o que esta explicitamente excluido?

2. **Requisitos Tecnicos**
   - Funcionalidade core necessaria
   - Interfaces voltadas ao usuario (API, UI, CLI, etc.)
   - Pontos de integracao com o codigo existente

3. **Plano de Implementacao**
   - Modulos/arquivos-chave a criar ou modificar
   - Exemplos simples de codigo mostrando a abordagem
   - Estruturas de dados e interfaces claras

4. **Criterios de Aceite**
   - Como saberemos que esta pronto?
   - Que funcionalidade especifica deve funcionar?
   - Que testes devem passar?

## 5. Fase de Criacao de Issues no GitHub

- Para cada especificacao, crie uma issue no GitHub:
  - Titulo claro e descritivo
  - Especificacao completa no corpo
  - Labels apropriadas (enhancement, high-priority, etc.)
  - Mencione explicitamente a filosofia MVP quando relevante

**Estrutura de Template da Issue:**

# [Feature Name]

## Overview
[Brief description of the feature and its purpose]

## Scope
[What's included and what's explicitly excluded]

## Technical Requirements
[Specific technical needs and constraints]

## Implementation Plan
[Step-by-step approach with simple code examples]

## Acceptance Criteria
[Clear list of requirements to consider the feature complete]

## Priority
[Justification for prioritization]

## Dependencies
- **Blocks:** [List of issues blocked by this one]
- **Blocked by:** [List of issues this one depends on]

## Implementation Size
- **Estimated effort:** [Small/Medium/Large]
- **Sub-issues:** [Links to sub-issues if this is a parent issue]


## 5.5 Work Distribution Optimization

- **Independence Analysis**
  - Revise cada especificacao para identificar componentes realmente independentes
  - Refatore especificacoes para maximizar work streams independentes
  - Crie limites claros entre componentes interdependentes

- **Dependency Mapping**
  - Para features com dependencias inevitaveis, estabeleca hierarquias claras
  - Crie issues pai para a feature geral com sub-issues por componente
  - Documente explicitamente relacoes "blocked by" e "blocks"

- **Workload Balancing**
  - Quebre especificacoes grandes em sub-issues menores e gerenciaveis
  - Garanta que cada sub-issue represente 1-3 dias de trabalho
  - Inclua criterios de aceite especificos por sub-issue

**Implementation Guidelines:**
- Use sintaxe de linking de issues do GitHub para criar relacoes explicitas
- Adicione labels indicando status de dependencia (ex.: "blocked", "prerequisite")
- Inclua estimativa de complexidade/esforco para cada issue

## 6. Fase de Revisao Final

- Resuma todas as especificacoes criadas
- Destaque dependencias de implementacao entre features
- Sugira uma ordem logica de implementacao
- Observe desafios ou consideracoes potenciais

Lembretes durante todo o processo:
- Favor simplicidade sobre complexidade
- Comece com implementacoes minimas viaveis que funcionem
- Foque na experiencia do dev
- Construa uma base que possa ser estendida depois
- Considere a comunidade open-source e o modelo de contribuicao

Este workflow materializa nossa abordagem e ajuda a manter consistencia na especificacao e priorizacao de features, garantindo que os projetos evoluam de forma cuidadosa e centrada no usuario.
