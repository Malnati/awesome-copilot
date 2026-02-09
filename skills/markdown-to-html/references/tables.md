# Organizing information with tables

Voce pode criar tabelas para organizar informacoes em comentarios, issues, pull requests e wikis.

## Creating a table

Voce pode criar tabelas com pipes `|` e hifens `-`. Os hifens sao usados para criar o header de cada coluna, enquanto pipes separam cada coluna. Voce deve incluir uma linha em branco antes da tabela para que ela seja renderizada corretamente.

```markdown

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
```

![Screenshot de uma tabela do GitHub Markdown renderizada com duas colunas iguais. Os cabecalhos aparecem em negrito e linhas alternadas ficam cinzas.](https://docs.github.com/assets/images/help/writing/table-basic-rendered.png)

Os pipes nas extremidades da tabela sao opcionais.

As celulas podem variar de largura e nao precisam estar perfeitamente alinhadas nas colunas. Deve haver pelo menos tres hifens em cada coluna do header.

```markdown
| Command | Description |
| --- | --- |
| git status | List all new or modified files |
| git diff | Show file differences that haven't been staged |
```

![Screenshot de uma tabela do GitHub Markdown com duas colunas de larguras diferentes. As linhas listam os comandos "git status" e "git diff" e suas descricoes.](https://docs.github.com/assets/images/help/writing/table-varied-columns-rendered.png)

Se voce edita snippets de codigo e tabelas com frequencia, pode ser util habilitar uma fonte fixed-width em todos os campos de comentario no GitHub. Para mais informacoes, veja [About writing and formatting on GitHub](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github#enabling-fixed-width-fonts-in-the-editor).

## Formatting content within your table

Voce pode usar [formatting](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) como links, inline code blocks e estilo de texto dentro da tabela:

```markdown
| Command | Description |
| --- | --- |
| `git status` | List all *new or modified* files |
| `git diff` | Show file differences that **haven't been** staged |
```

![Screenshot de uma tabela do GitHub Markdown com comandos formatados como code blocks. Negrito e italico sao usados nas descricoes.](https://docs.github.com/assets/images/help/writing/table-inline-formatting-rendered.png)

Voce pode alinhar o texto a esquerda, direita ou centro de uma coluna incluindo dois pontos `:` a esquerda, direita ou ambos os lados dos hifens no header.

```markdown
| Left-aligned | Center-aligned | Right-aligned |
| :---         |     :---:      |          ---: |
| git status   | git status     | git status    |
| git diff     | git diff       | git diff      |
```

![Screenshot de uma tabela Markdown renderizada no GitHub mostrando como o texto nas celulas pode ser alinhado a esquerda, centro ou direita.](https://docs.github.com/assets/images/help/writing/table-aligned-text-rendered.png)

Para incluir um pipe `|` como conteudo dentro da celula, use `\` antes do pipe:

```markdown
| Name     | Character |
| ---      | ---       |
| Backtick | `         |
| Pipe     | \|        |
```

![Screenshot de uma tabela Markdown renderizada no GitHub mostrando como pipes, que normalmente fecham celulas, aparecem quando precedidos por uma barra invertida.](https://docs.github.com/assets/images/help/writing/table-escaped-character-rendered.png)

## Further reading

* [GitHub Flavored Markdown Spec](https://github.github.com/gfm/)
* [Basic writing and formatting syntax](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
