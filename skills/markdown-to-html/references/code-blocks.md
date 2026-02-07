# Creating and highlighting code blocks

Compartilhe amostras de codigo com fenced code blocks e habilite syntax highlighting.

## Fenced code blocks

Voce pode criar fenced code blocks colocando tres backticks <code>```</code> antes e depois do bloco de codigo. Recomendamos colocar uma linha em branco antes e depois dos code blocks para facilitar a leitura da formatacao bruta.

````text
```
function test() {
  console.log("notice the blank line before this function?");
}
```
````

![Screenshot do GitHub Markdown renderizado mostrando o uso de tres backticks para criar code blocks. O bloco comeca com "function test() {.".](https://docs.github.com/assets/images/help/writing/fenced-code-block-rendered.png)

> [!TIP]
> To preserve your formatting within a list, make sure to indent non-fenced code blocks by eight spaces.

Para exibir tres backticks em um fenced code block, envolva-os dentro de quatro backticks.

`````text
````
```
Look! You can see my backticks.
```
````
`````

![Screenshot do Markdown renderizado mostrando que quando voce escreve tres backticks entre quatro backticks, eles ficam visiveis no conteudo renderizado.](https://docs.github.com/assets/images/help/writing/fenced-code-show-backticks-rendered.png)

Se voce edita snippets de codigo e tabelas com frequencia, pode ser util habilitar uma fonte fixed-width em todos os campos de comentario no GitHub. Para mais informacoes, veja [About writing and formatting on GitHub](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github#enabling-fixed-width-fonts-in-the-editor).

## Syntax highlighting

<!-- If you make changes to this feature, check whether any of the changes affect languages listed in /get-started/learning-about-github/github-language-support. If so, please update the language support article accordingly. -->

Voce pode adicionar um language identifier opcional para habilitar syntax highlighting no seu fenced code block.

Syntax highlighting muda a cor e o estilo do codigo-fonte para facilitar a leitura.

Por exemplo, para dar syntax highlighting em codigo Ruby:

````text
```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```
````

Isso exibira o code block com syntax highlighting:

![Screenshot de tres linhas de codigo Ruby exibidas no GitHub. Elementos do codigo aparecem em roxo, azul e vermelho para facilitar a leitura.](https://docs.github.com/assets/images/help/writing/code-block-syntax-highlighting-rendered.png)

> [!TIP]
> Quando voce cria um fenced code block que tambem precisa de syntax highlighting em um site GitHub Pages, use language identifiers em minusculo. Para mais informacoes, veja [About GitHub Pages and Jekyll](https://docs.github.com/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll#syntax-highlighting).

Usamos [Linguist](https://github.com/github-linguist/linguist) para deteccao de linguagem e para selecionar [third-party grammars](https://github.com/github-linguist/linguist/blob/main/vendor/README.md) para syntax highlighting. Voce pode descobrir quais keywords sao validas em [the languages YAML file](https://github.com/github-linguist/linguist/blob/main/lib/linguist/languages.yml).

## Creating diagrams

Voce tambem pode usar code blocks para criar diagramas em Markdown. O GitHub suporta Mermaid, GeoJSON, TopoJSON e ASCII STL. Para mais informacoes, veja [Creating diagrams](https://docs.github.com/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams).

## Further reading

* [GitHub Flavored Markdown Spec](https://github.github.com/gfm/)
* [Basic writing and formatting syntax](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
