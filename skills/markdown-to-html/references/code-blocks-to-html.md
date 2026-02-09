# Code Blocks para HTML

## Fenced Code Blocks (Sem Linguagem)

### Markdown

```
function test() {
  console.log("notice the blank line before this function?");
}
```

### HTML Parseado

```html
<pre><code>
function test() {
  console.log("notice the blank line before this function?");
}
</code></pre>
```

---

## GitHub Tip Callout

### Markdown

```md
> [!TIP]
> To preserve your formatting within a list, make sure to indent non-fenced code blocks by eight spaces.
```

### HTML Parseado (GitHub-specific)

```html
<blockquote class="markdown-alert markdown-alert-tip">
  <p><strong>Tip</strong></p>
  <p>To preserve your formatting within a list, make sure to indent non-fenced code blocks by eight spaces.</p>
</blockquote>
```

---

## Mostrar Backticks Dentro de Code Blocks

### Markdown

`````md
    ````
    ```
    Look! You can see my backticks.
    ```
    ````
`````

### HTML Parseado

```html
    <pre><code>
    ```

    Look! You can see my backticks.

    ```
    </code></pre>
```

## Syntax Highlighting (Language Identifier)

### Markdown

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

### HTML Parseado

```html
<pre><code class="language-ruby">
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
</code></pre>
```

> A classe `language-ruby` e consumida pelo syntax highlighter do GitHub (Linguist + grammar).

### Resumo: Regras de Syntax Highlighting (Nivel HTML)

| Markdown fence | Parsed `<code>` tag            |
| -------------- | ------------------------------ |
| ```js          | `<code class="language-js">`   |
| ```html        | `<code class="language-html">` |
| ```md          | `<code class="language-md">`   |
| ``` (no lang)  | `<code>`                       |

---

## HTML Comments (Ignored by Renderer)

```md
<!-- Internal documentation comment -->
```

```html
<!-- Internal documentation comment -->
```

---

## Links

```md
[About writing and formatting on GitHub](https://docs.github.com/...)
```

```html
<a href="https://docs.github.com/...">About writing and formatting on GitHub</a>
```

---

## Lists

```md
* [GitHub Flavored Markdown Spec](https://github.github.com/gfm/)
```

```html
<ul>
  <li>
    <a href="https://github.github.com/gfm/">GitHub Flavored Markdown Spec</a>
  </li>
</ul>
```

---

## Diagrams (Conceptual Parsing)

### Markdown

````md
```mermaid
graph TD
  A --> B
```
````

### HTML Parseado

```html
<pre><code class="language-mermaid">
graph TD
  A --> B
</code></pre>
```

## Closing Notes

* Nenhuma classe `language-*` aparece aqui porque **nenhum language identifier** foi fornecido.
* Os tres backticks internos sao preservados **como texto literal** dentro de `<code>`.
