---
name: vscode-ext-commands
description: 'Diretrizes para contribuir comandos em extensoes do VS Code. Indica convencao de nomeacao, visibilidade, localizacao e outros atributos relevantes, seguindo diretrizes de desenvolvimento de extensoes do VS Code, bibliotecas e boas praticas'
---

# Contribuicao de comandos em extensoes do VS Code

Esta skill ajuda voce a contribuir comandos em extensoes do VS Code

## Quando usar esta skill

Use esta skill quando precisar:
- Adicionar ou atualizar comandos na sua extensao do VS Code

# Instrucoes

Comandos do VS Code sempre devem definir um `title`, independente de categoria, visibilidade ou localizacao. Usamos alguns padroes para cada "tipo" de comando, com caracteristicas descritas abaixo:

* Comandos regulares: Por padrao, todos os comandos devem estar acessiveis no Command Palette, devem definir `category`, e nao precisam de `icon`, a menos que o comando seja usado na Side Bar.

* Comandos de Side Bar: O nome segue um padrao especial, comecando com underscore (`_`) e sufixo `#sideBar`, como `_extensionId.someCommand#sideBar`. Deve definir um `icon` e pode ou nao ter alguma regra de `enablement`. Comandos exclusivos da Side Bar nao devem ser visiveis no Command Palette. Ao contribuir em `view/title` ou `view/item/context`, devemos informar a _order/position_ em que sera exibido, e podemos usar termos "relative to other command/button" para identificar o `group` correto. Tambem e boa pratica definir a condicao (`when`) para o novo comando ficar visivel.
