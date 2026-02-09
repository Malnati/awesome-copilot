---
agent: 'agent'
description: 'Crie issues no GitHub para requisitos nao implementados a partir de arquivos de especificacao usando o template feature_request.yml.'
tools: ['search/codebase', 'search', 'github', 'create_issue', 'search_issues', 'update_issue']
---
# Criar Issues no GitHub para Requisitos de Especificacao nao Atendidos

Crie issues no GitHub para requisitos nao implementados na especificacao em `${file}`.

## Processo

1. Analise o arquivo de especificacao para extrair todos os requisitos
2. Verifique o status de implementacao no codebase para cada requisito
3. Busque issues existentes com `search_issues` para evitar duplicatas
4. Crie uma issue nova por requisito nao implementado usando `create_issue`
5. Use o template `feature_request.yml` (fallback para o default)

## Requisitos

- Uma issue por requisito nao implementado da especificacao
- Mapeamento claro de ID do requisito e descricao
- Inclua orientacao de implementacao e criterios de aceite
- Verifique contra issues existentes antes de criar

## Conteudo da Issue

- Titulo: ID do requisito e descricao breve
- Descricao: Requisito detalhado, metodo de implementacao e contexto
- Labels: feature, enhancement (conforme apropriado)

## Checagem de Implementacao

- Procure no codebase por patterns de codigo relacionados
- Verifique arquivos de especificacao em `/spec/`
- Confirme que o requisito nao esta parcialmente implementado
