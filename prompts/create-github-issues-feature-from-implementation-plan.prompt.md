---
agent: 'agent'
description: 'Crie GitHub Issues a partir de fases do plano de implementacao usando templates feature_request.yml ou chore_request.yml.'
tools: ['search/codebase', 'search', 'github', 'create_issue', 'search_issues', 'update_issue']
---
# Criar GitHub Issue a partir de Plano de Implementacao

Crie GitHub Issues para o plano de implementacao em `${file}`.

## Processo

1. Analise o arquivo do plano para identificar fases
2. Verifique issues existentes usando `search_issues`
3. Crie nova issue por fase usando `create_issue` ou atualize existente com `update_issue`
4. Use os templates `feature_request.yml` ou `chore_request.yml` (fallback para default)

## Requisitos

- Uma issue por fase de implementacao
- Titulos e descricoes claros e estruturados
- Inclua apenas mudancas exigidas pelo plano
- Verifique contra issues existentes antes de criar

## Conteudo da Issue

- Title: Nome da fase do plano de implementacao
- Description: Detalhes da fase, requisitos e contexto
- Labels: Apropriadas ao tipo de issue (feature/chore)
