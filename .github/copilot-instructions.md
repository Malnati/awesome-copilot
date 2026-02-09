As instrucoes a seguir devem ser aplicadas somente ao realizar code review.

## Atualizacoes do README

- [ ] O novo arquivo deve ser adicionado ao `docs/README.<type>.md`.

## Guia de arquivo de prompt

**Aplique apenas a arquivos que terminem em `.prompt.md`**

- [ ] O prompt tem front matter markdown.
- [ ] O prompt tem um campo `agent` especificado como `agent`, `ask` ou `Plan`.
- [ ] O prompt tem um campo `description`.
- [ ] O campo `description` nao esta vazio.
- [ ] O nome do arquivo esta em minusculas, com palavras separadas por hifens.
- [ ] Incentive o uso de `tools`, mas nao e obrigatorio.
- [ ] Incentive fortemente o uso de `model` para especificar o modelo para o qual o prompt e otimizado.
- [ ] Incentive fortemente o uso de `name` para definir o nome do prompt.

## Guia de arquivo de instruction

**Aplique apenas a arquivos que terminem em `.instructions.md`**

- [ ] A instruction tem front matter markdown.
- [ ] A instruction tem um campo `description`.
- [ ] O campo `description` nao esta vazio.
- [ ] O nome do arquivo esta em minusculas, com palavras separadas por hifens.
- [ ] A instruction tem um campo `applyTo` que especifica o(s) arquivo(s) aos quais as instrucoes se aplicam. Se desejar especificar multiplos caminhos de arquivo, devem ser formatados como `'**.js, **.ts'`.

## Guia de arquivo de agent

**Aplique apenas a arquivos que terminem em `.agent.md`**

- [ ] O agent tem front matter markdown.
- [ ] O agent tem um campo `description`.
- [ ] O campo `description` nao esta vazio.
- [ ] O nome do arquivo esta em minusculas, com palavras separadas por hifens.
- [ ] Incentive o uso de `tools`, mas nao e obrigatorio.
- [ ] Incentive fortemente o uso de `model` para especificar o modelo para o qual o agent e otimizado.
- [ ] Incentive fortemente o uso de `name` para definir o nome do agent.

## Guia de Agent Skills

**Aplique apenas a pastas no diretorio `skills/`**

- [ ] A pasta de skill contem um arquivo `SKILL.md`.
- [ ] O `SKILL.md` tem front matter markdown.
- [ ] O `SKILL.md` tem um campo `name`.
- [ ] O valor do campo `name` e em minusculas com palavras separadas por hifens.
- [ ] O campo `name` corresponde ao nome da pasta.
- [ ] O `SKILL.md` tem um campo `description`.
- [ ] O campo `description` nao esta vazio, tem pelo menos 10 caracteres e no maximo 1024 caracteres.
- [ ] O valor do campo `description` esta envolto em aspas simples.
- [ ] O nome da pasta esta em minusculas, com palavras separadas por hifens.
- [ ] Quaisquer ativos empacotados (scripts, templates, arquivos de dados) sao referenciados nas instrucoes do `SKILL.md`.
- [ ] Ativos empacotados tem tamanho razoavel (menos de 5MB por arquivo).

## Guia de arquivo de collection

**Aplique apenas a arquivos que terminem em `.collection.yml`**

- [ ] A collection tem um campo `name`.
- [ ] A collection tem um campo `description`.
- [ ] O campo `description` nao esta vazio.
- [ ] A collection tem um campo `tags`.
- [ ] O nome do arquivo esta em minusculas, com palavras separadas por hifens.
- [ ] Cada item na collection tem um campo `path`.
- [ ] Cada item na collection tem um campo `kind`.
- [ ] O valor do campo `kind` e um de: `prompt`, `instruction`, `agent` ou `skill`.
- [ ] A collection nao inclui itens duplicados.
- [ ] A collection nao referencia arquivos inexistentes.
- [ ] Cada item pode ter um campo `usage` opcional descrevendo quando usar o item.
