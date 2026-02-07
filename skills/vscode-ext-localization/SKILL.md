---
name: vscode-ext-localization
description: 'Diretrizes para localizacao adequada de extensoes do VS Code, seguindo diretrizes de desenvolvimento de extensoes do VS Code, bibliotecas e boas praticas'
---

# Localizacao de extensoes do VS Code

Esta skill ajuda voce a localizar todos os aspectos de extensoes do VS Code

## Quando usar esta skill

Use esta skill quando precisar:
- Localizar configuracoes (settings), comandos, menus, views ou walkthroughs novos ou existentes
- Localizar mensagens ou outras strings no codigo da extensao (JavaScript ou TypeScript) exibidas ao usuario final

# Instrucoes

A localizacao no VS Code e composta por tres abordagens diferentes, dependendo do recurso que esta sendo localizado. Quando um novo recurso localizavel e criado ou atualizado, a localizacao correspondente para todos os idiomas atualmente disponiveis deve ser criada/atualizada.

1. Configuracoes como Settings, Commands, Menus, Views, ViewsWelcome, Walkthrough Titles e Descriptions, definidas em `package.json`
  -> Um arquivo exclusivo `package.nls.LANGID.json`, como `package.nls.pt-br.json` para localizacao em Portugues do Brasil (`pt-br`)
2. Conteudo de Walkthrough (definido em seus proprios arquivos `Markdown`)
  -> Um arquivo `Markdown` exclusivo como `walkthrough/someStep.pt-br.md` para localizacao em Portugues do Brasil
3. Mensagens e strings localizadas no codigo da extensao (arquivos JavaScript ou TypeScript)
  -> Um arquivo exclusivo `bundle.l10n.pt-br.json` para localizacao em Portugues do Brasil
