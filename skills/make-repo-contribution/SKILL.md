---
name: make-repo-contribution
description: 'Todas as mudancas no codigo devem seguir a orientacao documentada no repositorio. Antes de qualquer issue ser criada, branch ser criada, commits gerados ou pull request (PR) criado, uma busca deve ser feita para garantir que os passos corretos sejam seguidos. Sempre que for solicitado criar uma issue, mensagens de commit, fazer push de codigo ou criar um PR, use esta skill para que tudo seja feito corretamente.'
---

# Diretrizes de contribuicao

A maioria dos projetos possui um conjunto de diretrizes de contribuicao que todos precisam seguir ao criar issues, pull requests (PR) ou contribuir codigo. Isso pode incluir, mas nao se limita a:

- Criar uma issue antes de criar um PR, ou criar os dois em conjunto
- Templates para issues ou PRs que devem ser usados dependendo da mudanca solicitada
- Diretrizes sobre o que precisa ser documentado nesses issues e PRs
- Testes, linters e outros pre-requisitos que precisam ser executados antes de enviar mudancas

Lembre-se sempre: voce e um convidado no repositorio de outra pessoa. Portanto, siga as regras e diretrizes definidas pelo owner ao contribuir codigo.

## Usando diretrizes existentes

Antes de criar um PR ou qualquer etapa que o anteceda, explore o projeto para determinar se ha orientacao. Locais para explorar incluem, mas nao se limitam a:

- README.md
- CONTRIBUTING.md
- Documentacao do projeto
- Templates de issue
- Templates de pull request ou PR

Se algum desses existir, ou se voce encontrar documentacao em outro lugar do repo, leia o que encontrar, considere e siga as diretrizes o melhor possivel. Se tiver perguntas ou duvidas, pergunte ao usuario como prosseguir. NAO crie um PR ate ter certeza de que seguiu as praticas.

## Nenhuma diretriz encontrada

Se nenhuma diretriz for encontrada, ou nao cobrir certos topicos, use o seguinte como base para criar uma contribuicao de qualidade. **SEMPRE** priorize a orientacao fornecida pelo repositorio.

## Tarefas

Muitos owners de repositorios terao orientacao sobre pre-requisitos que precisam ser concluidos antes de um PR. Isso pode incluir, mas nao se limita a:

- buildar o projeto ou gerar assets
- rodar linters e garantir que quaisquer issues estejam resolvidas
- diretrizes de nomeacao e outros patterns
- testes unitarios, end-to-end ou outros testes que precisam ser criados e passar
  - relacionado a isso, pode haver porcentagens de cobertura obrigatorias

Revise toda orientacao encontrada e garanta que quaisquer pre-requisitos foram atendidos.

## Issue

Sempre comece verificando se existe uma issue relacionada a tarefa. Ela pode ja ter sido criada pelo usuario ou por outra pessoa. Se encontrar uma, pergunte ao usuario se ele quer usá-la ou qual deseja usar.

Se nenhuma issue for encontrada, verifique se criar uma issue e um requisito. Se for, use o template fornecido no repositorio. Se houver varios, escolha o que mais se alinha ao trabalho. Se houver duvidas, pergunte ao usuario qual usar.

Se a exigencia for criar uma issue, mas nenhum template for fornecido, use [este template de issue](./assets/issue-template.md) como guia.

## Branch

Antes de fazer commits, garanta que uma branch foi criada para o trabalho. Siga a orientacao do repositorio. Se houver prefixos definidos, como `feature` ou `chore`, ou se o requisito for usar o username de quem cria o PR, entao use isso. Esta branch nunca deve ser `main` ou a branch default, e sim uma branch criada especificamente para as mudancas em andamento. Se nenhuma branch tiver sido criada, crie uma nova com um bom nome baseado nas mudancas e na orientacao.

## Commits

Ao commitar mudancas:

1. Revise todas as mudancas
2. Agrupe as mudancas logicamente
3. Crie mensagens de commit curtas para cada grupo, seguindo a orientacao do repositorio
4. Commite o codigo agrupado na branch

## Merge

**NUNCA** faca merge na main a menos que o usuario instrua explicitamente.

## Pull request

Ao criar um pull request, use templates existentes no repositorio, se houver, seguindo a orientacao encontrada.

Se nenhum template for fornecido, use [este template de PR](./assets/pr-template.md). Ele contem uma colecao de cabecalhos a usar, cada um com orientacao sobre o que colocar em cada secao.

Se uma issue foi criada ou esta sendo usada, garanta que essa issue seja referenciada no PR. Use a sintaxe `Closes #NUMBER` para habilitar o fechamento automatico da issue.
