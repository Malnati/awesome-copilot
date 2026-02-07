---
agent: 'agent'
description: 'Crie Pull Request no GitHub para request de feature a partir de arquivo de especificacao usando o template pull_request_template.md.'
tools: ['search/codebase', 'search', 'github', 'create_pull_request', 'update_pull_request', 'get_pull_request_diff']
---
# Criar Pull Request no GitHub a partir de Especificacao

Crie um Pull Request no GitHub para a especificacao em `${workspaceFolder}/.github/pull_request_template.md`.

## Processo

1. Analise o template de especificacao em `${workspaceFolder}/.github/pull_request_template.md` para extrair requisitos usando a ferramenta `search`.
2. Crie um rascunho de pull request usando a ferramenta `create_pull_request` para `${input:targetBranch}` e garanta que nao exista um pull request atual para o branch usando `get_pull_request`. Se existir, continue para o passo 4 e pule o passo 3.
3. Obtenha as mudancas no pull request usando `get_pull_request_diff` para analisar o que foi alterado no PR.
4. Atualize o corpo e o titulo do pull request criado no passo anterior usando a ferramenta `update_pull_request`. Incorpore as informacoes do template obtido no passo 1 para ajustar corpo e titulo conforme necessario.
5. Troque de draft para ready for review usando `update_pull_request` para atualizar o estado do pull request.
6. Use `get_me` para obter o username de quem criou o pull request e atribua via `update_issue`.
7. Responda ao usuario com a URL do pull request criado.

## Requisitos
- Um unico pull request para a especificacao completa
- Titulo claro identificando a especificacao em pull_request_template.md
- Preencher informacao suficiente no pull_request_template.md
- Verificar contra pull requests existentes antes de criar
