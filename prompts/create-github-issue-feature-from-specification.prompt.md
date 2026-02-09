---
agent: 'agent'
description: 'Criar Issue no GitHub para solicitação de feature a partir de arquivo de especificação usando o template feature_request.yml.'
tools: ['search/codebase', 'search', 'github', 'create_issue', 'search_issues', 'update_issue']
---
## Criar Issue no GitHub a partir de Especificação

Crie uma Issue no GitHub para a especificação em `${file}`.

## Processo

1. Analise o arquivo de especificação para extrair requisitos
2. Verifique issues existentes usando `search_issues`
3. Crie nova issue usando `create_issue` ou atualize existente com `update_issue`
4. Use o template `feature_request.yml` (fallback para o padrão)

## Requisitos

- Uma única issue para toda a especificação
- Título claro identificando a especificação
- Inclua apenas mudanças requeridas pela especificação
- Verifique contra issues existentes antes de criar

## Conteúdo da Issue

- Título: Nome da feature da especificação
- Descrição: Problema, solução proposta e contexto
- Labels: feature, enhancement (conforme apropriado)
