---
name: markdown-to-html
description: 'Converta arquivos Markdown para HTML semelhante a `marked.js`, `pandoc`, `gomarkdown/markdown` ou ferramentas similares; ou escreva script customizado para converter markdown para html e/ou trabalhar em sistemas de template web como `jekyll/jekyll`, `gohugoio/hugo` ou similares que utilizam documentos markdown convertendo-os para html. Use quando solicitarem "convert markdown to html", "transform md to html", "render markdown", "generate html from markdown" ou ao trabalhar com arquivos .md e/ou sistemas de template que convertem markdown em HTML. Suporta workflows CLI e Node.js com GFM, CommonMark e variantes padrão de Markdown.'
---

# Conversao de Markdown para HTML

Skill especialista para converter documentos Markdown em HTML usando a biblioteca marked.js, ou escrever scripts de conversao de dados; neste caso scripts similares ao repositorio [markedJS/marked](https://github.com/markedjs/marked). Para scripts customizados, o conhecimento nao se limita a `marked.js`, mas utiliza metodos de conversao de dados de ferramentas como [pandoc](https://github.com/jgm/pandoc) e [gomarkdown/markdown](https://github.com/gomarkdown/markdown); [jekyll/jekyll](https://github.com/jekyll/jekyll) e [gohugoio/hugo](https://github.com/gohugoio/hugo) para sistemas de template.

O script ou ferramenta (tool) de conversao deve lidar com arquivos individuais, conversoes em lote e configuracoes avancadas.

## Quando Usar Esta Skill

- Usuario pede para "convert markdown to html" ou "transform md files"
- Usuario quer "render markdown" como HTML
- Usuario precisa gerar documentacao HTML a partir de arquivos .md
- Usuario esta construindo sites estaticos a partir de conteudo Markdown
- Usuario esta construindo sistema de template que converte markdown para html
- Usuario esta trabalhando em uma tool, widget ou template customizado para um sistema existente
- Usuario quer visualizar Markdown renderizado como HTML

## Convertendo Markdown para HTML

### Conversoes Basicas Essenciais

Para mais, veja [basic-markdown-to-html.md](references/basic-markdown-to-html.md)

```text
    ```markdown
    # Level 1
    ## Level 2

    One sentence with a [link](https://example.com), and a HTML snippet like `<p>paragraph tag</p>`.

    - `ul` list item 1
    - `ul` list item 2

    1. `ol` list item 1
    2. `ol` list item 1

    | Item da Tabela | Descricao |
    | One | One is the spelling of the number `1`. |
    | Two | Two is the spelling of the number `2`. |

    ```js
    var one = 1;
    var two = 2;

    function simpleMath(x, y) {
     return x + y;
    }
    console.log(simpleMath(one, two));
    ```
    ```

    ```html
    <h1>Level 1</h1>
    <h2>Level 2</h2>

    <p>One sentence with a <a href="https://example.com">link</a>, and a HTML snippet like <code>&lt;p&gt;paragraph tag&lt;/p&gt;</code>.</p>

    <ul>
     <li>`ul` list item 1</li>
     <li>`ul` list item 2</li>
    </ul>

    <ol>
     <li>`ol` list item 1</li>
     <li>`ol` list item 2</li>
    </ol>

    <table>
     <thead>
      <tr>
       <th>Item da Tabela</th>
       <th>Descricao</th>
      </tr>
     </thead>
     <tbody>
      <tr>
       <td>One</td>
       <td>One is the spelling of the number `1`.</td>
      </tr>
      <tr>
       <td>Two</td>
       <td>Two is the spelling of the number `2`.</td>
      </tr>
     </tbody>
    </table>

    <pre>
     <code>var one = 1;
     var two = 2;

     function simpleMath(x, y) {
      return x + y;
     }
     console.log(simpleMath(one, two));</code>
    </pre>
    ```
```

### Conversoes de Blocos de Codigo

Para mais, veja [code-blocks-to-html.md](references/code-blocks-to-html.md)

```text

    ```markdown
    your code here
    ```

    ```html
    <pre><code class="language-md">
    your code here
    </code></pre>
    ```

    ```js
    console.log("Hello world");
    ```

    ```html
    <pre><code class="language-js">
    console.log("Hello world");
    </code></pre>
    ```

    ```markdown
      ```

      ```
      visible backticks
      ```

      ```
    ```

    ```html
      <pre><code>
      ```

      visible backticks

      ```
      </code></pre>
    ```
```

### Conversoes de Secoes Colapsadas

Para mais, veja [collapsed-sections-to-html.md](references/collapsed-sections-to-html.md)

```text
    ```markdown
    <details>
    <summary>More info</summary>

    ### Header inside

    - Lists
    - **Formatacao**
    - Code blocks

        ```js
        console.log("Hello");
        ```

    </details>
    ```

    ```html
    <details>
    <summary>More info</summary>

    <h3>Header inside</h3>

    <ul>
     <li>Lists</li>
     <li><strong>Formatting</strong></li>
     <li>Code blocks</li>
    </ul>

    <pre>
     <code class="language-js">console.log("Hello");</code>
    </pre>

    </details>
    ```
```

### Conversoes de Expressoes Matematicas

Para mais, veja [writing-mathematical-expressions-to-html.md](references/writing-mathematical-expressions-to-html.md)

```text
    ```markdown
    This sentence uses `$` delimiters to show math inline: $\sqrt{3x-1}+(1+x)^2$
    ```
