# Sintaxe basica de escrita e formatacao

Crie formatacao sofisticada para sua prosa e codigo no GitHub com sintaxe simples.

## Titulos

Para criar um titulo, adicione de um a seis simbolos <kbd>#</kbd> antes do texto. O numero de <kbd>#</kbd> determina o nivel hierarquico e o tamanho da fonte do titulo.

```markdown
# A first-level heading
## A second-level heading
### A third-level heading
```

![Screenshot do GitHub Markdown renderizado mostrando exemplos de cabecalhos h1, h2 e h3, que diminuem em tamanho e peso visual para indicar hierarquia.](https://docs.github.com/assets/images/help/writing/headings-rendered.png)

Quando voce usa dois ou mais titulos, o GitHub gera automaticamente um table of contents que pode ser acessado ao clicar no icone de menu "Outline" <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-list-unordered" aria-label="Table of Contents" role="img"><path d="M5.75 2.5h8.5a.75.75 0 0 1 0 1.5h-8.5a.75.75 0 0 1 0-1.5Zm0 5h8.5a.75.75 0 0 1 0 1.5h-8.5a.75.75 0 0 1 0-1.5Zm0 5h8.5a.75.75 0 0 1 0 1.5h-8.5a.75.75 0 0 1 0-1.5ZM2 14a1 1 0 1 1 0-2 1 1 0 0 1 0 2Zm1-6a1 1 0 1 1-2 0 1 1 0 0 1 2 0ZM2 4a1 1 0 1 1 0-2 1 1 0 0 1 0 2Z"></path></svg> no header do arquivo. Cada titulo aparece no table of contents e voce pode clicar para navegar ate a secao escolhida.

![Screenshot de um arquivo README com o menu suspenso do table of contents aberto. O icone de table of contents esta contornado em laranja escuro.](https://docs.github.com/assets/images/help/repository/headings-toc.png)

## Styling text

Voce pode indicar enfase com texto em negrito, italico, tachado, subscript ou superscript em campos de comentario e arquivos `.md`.

| Estilo                 | Sintaxe             | Atalho de teclado                                                                     | Exemplo                                  | Saida                                  |                                                   |
| ---------------------- | ------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------- | -------------------------------------- | ------------------------------------------------- |
| Bold                   | `** **` or `__ __`  | <kbd>Command</kbd>+<kbd>B</kbd> (Mac) or <kbd>Ctrl</kbd>+<kbd>B</kbd> (Windows/Linux) | `**This is bold text**`                  | **This is bold text**                  |                                                   |
| Italic                 | `* *` or `_ _`      | <kbd>Command</kbd>+<kbd>I</kbd> (Mac) or <kbd>Ctrl</kbd>+<kbd>I</kbd> (Windows/Linux) | `_This text is italicized_`              | *This text is italicized*              |                                                   |
| Strikethrough          | `~~ ~~` or `~ ~`    | None                                                                                  | `~~This was mistaken text~~`             | ~~This was mistaken text~~             |                                                   |
| Bold and nested italic | `** **` and `_ _`   | None                                                                                  | `**This text is _extremely_ important**` | **This text is *extremely* important** |                                                   |
| All bold and italic    | `*** ***`           | None                                                                                  | `***All this text is important***`       | ***All this text is important***       | <!-- markdownlint-disable-line emphasis-style --> |
| Subscript              | `<sub> </sub>`      | None                                                                                  | `This is a <sub>subscript</sub> text`    | This is a <sub>subscript</sub> text    |                                                   |
| Superscript            | `<sup> </sup>`      | None                                                                                  | `This is a <sup>superscript</sup> text`  | This is a <sup>superscript</sup> text  |                                                   |
| Underline              | `<ins> </ins>`      | None                                                                                  | `This is an <ins>underlined</ins> text`  | This is an <ins>underlined</ins> text  |                                                   |

## Quoting text

Voce pode citar texto com <kbd>></kbd>.

```markdown
Text that is not a quote

> Text that is a quote
```

Texto citado e recuado com uma linha vertical a esquerda e exibido em cinza.

![Screenshot do GitHub Markdown renderizado mostrando a diferenca entre texto normal e texto citado.](https://docs.github.com/assets/images/help/writing/quoted-text-rendered.png)

> [!NOTE]
> Ao visualizar uma conversa, voce pode citar texto em um comentario automaticamente selecionando o texto e pressionando <kbd>R</kbd>. Voce pode citar um comentario inteiro clicando no <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-kebab-horizontal" aria-label="The horizontal kebab icon" role="img"><path d="M8 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3ZM1.5 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Zm13 0a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Z"></path></svg> e depois em **Quote reply**. Para mais informacoes sobre atalhos de teclado, veja [Keyboard shortcuts](https://docs.github.com/en/get-started/accessibility/keyboard-shortcuts).

## Quoting code

Voce pode destacar codigo ou um comando dentro de uma frase usando backticks simples. O texto dentro dos backticks nao sera formatado. Voce tambem pode usar o atalho <kbd>Command</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para inserir backticks para um code block em uma linha de Markdown.

```markdown
Use `git status` to list all new or modified files that haven't yet been committed.
```

![Screenshot do GitHub Markdown renderizado mostrando que caracteres entre backticks aparecem em fonte de largura fixa e realce cinza claro.](https://docs.github.com/assets/images/help/writing/inline-code-rendered.png)

Para formatar codigo ou texto em seu proprio bloco distinto, use tres backticks.

````markdown
Some basic Git commands are:
```
git status
git add
git commit
```
````

![Screenshot do GitHub Markdown renderizado mostrando um code block simples sem syntax highlighting.](https://docs.github.com/assets/images/help/writing/code-block-rendered.png)

Para mais informacoes, veja [Creating and highlighting code blocks](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks).

Se voce edita snippets de codigo e tabelas com frequencia, pode ser util habilitar uma fonte fixed-width em todos os campos de comentario no GitHub. Para mais informacoes, veja [About writing and formatting on GitHub](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github#enabling-fixed-width-fonts-in-the-editor).

## Supported color models

Em issues, pull requests e discussions, voce pode destacar cores dentro de uma frase usando backticks. Um modelo de cor suportado dentro de backticks exibira uma visualizacao da cor.

```markdown
The background color is `#ffffff` for light mode and `#000000` for dark mode.
```

![Screenshot do GitHub Markdown renderizado mostrando como valores HEX dentro de backticks criam pequenos circulos de cor, aqui branco e depois preto.](https://docs.github.com/assets/images/help/writing/supported-color-models-rendered.png)

Aqui estao os modelos de cor atualmente suportados.

| Cor | Sintaxe                      | Exemplo                             | Saida                                                                                                                                                                         |
| ----- | --------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| HEX   | <code>`#RRGGBB`</code>    | <code>`#0969DA`</code>            | ![Screenshot do GitHub Markdown renderizado mostrando como o valor HEX #0969DA aparece com um circulo azul.](https://docs.github.com/assets/images/help/writing/supported-color-models-hex-rendered.png)       |
| RGB   | <code>`rgb(R,G,B)`</code> | <code>`rgb(9, 105, 218)`</code>   | ![Screenshot do GitHub Markdown renderizado mostrando como o valor RGB 9, 105, 218 aparece com um circulo azul.](https://docs.github.com/assets/images/help/writing/supported-color-models-rgb-rendered.png)   |
| HSL   | <code>`hsl(H,S,L)`</code> | <code>`hsl(212, 92%, 45%)`</code> | ![Screenshot do GitHub Markdown renderizado mostrando como o valor HSL 212, 92%, 45% aparece com um circulo azul.](https://docs.github.com/assets/images/help/writing/supported-color-models-hsl-rendered.png) |

> [!NOTE]
>
> * Um modelo de cor suportado nao pode ter espacos no inicio ou no fim dentro dos backticks.
> * A visualizacao da cor e suportada apenas em issues, pull requests e discussions.

## Links

Voce pode criar um link inline colocando o texto entre colchetes `[ ]` e a URL entre parenteses `( )`. Voce tambem pode usar o atalho <kbd>Command</kbd>+<kbd>K</kbd> para criar um link. Quando voce tiver texto selecionado, pode colar uma URL para criar automaticamente um link a partir da selecao.

Voce tambem pode criar um hyperlink Markdown destacando o texto e usando o atalho <kbd>Command</kbd>+<kbd>V</kbd>. Se quiser substituir o texto pelo link, use o atalho <kbd>Command</kbd>+<kbd>Shift</kbd>+<kbd>V</kbd>.

`This site was built using [GitHub Pages](https://pages.github.com/).`

![Screenshot do GitHub Markdown renderizado mostrando como o texto entre colchetes, "GitHub Pages", aparece como hyperlink azul.](https://docs.github.com/assets/images/help/writing/link-rendered.png)

> [!NOTE]
> O GitHub cria links automaticamente quando URLs validas sao escritas em um comentario. Para mais informacoes, veja [Autolinked references and URLs](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/autolinked-references-and-urls).

## Section links

Voce pode linkar diretamente para qualquer secao que tenha um heading. Para ver o anchor gerado automaticamente em um arquivo renderizado, passe o mouse sobre o heading para exibir o icone <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-link" aria-label="the link" role="img"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg> e clique no icone para ver o anchor no browser.

![Screenshot de um README de repositorio. A esquerda de um heading de secao, um icone de link esta contornado em laranja escuro.](https://docs.github.com/assets/images/help/repository/readme-links.png)

Se voce precisar determinar o anchor de um heading no arquivo que esta editando, pode usar as regras basicas abaixo:

* Letras sao convertidas para minusculas.
* Espacos sao substituidos por hifens (`-`). Outros caracteres de espaco ou pontuacao sao removidos.
* Espacos no inicio e no fim sao removidos.
* Formatacao de markup e removida, deixando apenas o conteudo (por exemplo, `_italics_` vira `italics`).
* Se o anchor gerado automaticamente para um heading for identico ao de um heading anterior no mesmo documento, um identificador unico sera gerado ao anexar um hifen e um inteiro auto-incrementado.

Para informacoes mais detalhadas sobre requisitos de URI fragments, veja [RFC 3986: Uniform Resource Identifier (URI): Generic Syntax, Section 3.5](https://www.rfc-editor.org/rfc/rfc3986#section-3.5).

O bloco de codigo abaixo demonstra as regras basicas usadas para gerar anchors a partir de headings no conteudo renderizado.

```markdown
# Example headings

## Sample Section

## This'll be a _Helpful_ Section About the Greek Letter Θ!
A heading containing characters not allowed in fragments, UTF-8 characters, two consecutive spaces between the first and second words, and formatting.

## This heading is not unique in the file

TEXT 1

## This heading is not unique in the file

TEXT 2

# Links to the example headings above

Link to the sample section: [Link Text](#sample-section).

Link to the helpful section: [Link Text](#thisll-be-a-helpful-section-about-the-greek-letter-Θ).

Link to the first non-unique section: [Link Text](#this-heading-is-not-unique-in-the-file).

Link to the second non-unique section: [Link Text](#this-heading-is-not-unique-in-the-file-1).
```

> [!NOTE]
> Se voce editar um heading, ou se alterar a ordem de headings com anchors "identicos", voce tambem precisara atualizar os links para esses headings, pois os anchors mudarao.

## Relative links

Voce pode definir links relativos e paths de imagem nos arquivos renderizados para ajudar leitores a navegar por outros arquivos no repositorio.

Um link relativo e um link que e relativo ao arquivo atual. Por exemplo, se voce tem um README na raiz e outro arquivo em *docs/CONTRIBUTING.md*, o link relativo para *CONTRIBUTING.md* no README pode ser algo como:

```text
[Contribution guidelines for this project](docs/CONTRIBUTING.md)
```

O GitHub transformara automaticamente o link relativo ou path de imagem com base no branch atual, para que o link ou path sempre funcione. O path do link sera relativo ao arquivo atual. Links que comecam com `/` sao relativos a raiz do repositorio. Voce pode usar todos os operandos de link relativo, como `./` e `../`.

O texto do link deve ficar em uma unica linha. O exemplo abaixo nao funciona.

```markdown
[Contribution
guidelines for this project](docs/CONTRIBUTING.md)
```

Links relativos sao mais faceis para usuarios que clonam o repositorio. Links absolutos podem nao funcionar em clones do repositorio - recomendamos usar links relativos para referenciar outros arquivos no repositorio.

## Custom anchors

Voce pode usar tags HTML de anchor padrao (`<a name="unique-anchor-name"></a>`) para criar pontos de navegacao no documento. Para evitar referencias ambiguas, use um esquema de nomenclatura unico, como adicionar um prefixo ao valor do atributo `name`.

> [!NOTE]
> Custom anchors nao serao incluidos no outline/Table of Contents do documento.

Voce pode linkar para um custom anchor usando o valor do atributo `name` que voce deu ao anchor. A sintaxe e exatamente a mesma de quando voce linka para um anchor gerado automaticamente para um heading.

Por exemplo:

```markdown
# Section Heading

Some body text of this section.
```