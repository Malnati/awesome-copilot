# Secoes Colapsadas para HTML

## Bloco `<details>` (Raw HTML em Markdown)

### Markdown

````md
<details>

<summary>Tips for collapsed sections</summary>

### You can add a header

You can add text within a collapsed section.

You can add an image or a code block, too.

    ```ruby
    puts "Hello World"
    ```

</details>
````

---

### HTML Parseado

```html
<details>
  <summary>Tips for collapsed sections</summary>

  <h3>You can add a header</h3>

  <p>You can add text within a collapsed section.</p>

  <p>You can add an image or a code block, too.</p>

  <pre><code class="language-ruby">
puts "Hello World"
</code></pre>
</details>
```

#### Notas:

* Markdown **dentro de `<details>`** ainda e parseado normalmente.
* Syntax highlighting e preservado via `class="language-ruby"`.

---

## Aberto por Padrao (atributo `open`)

### Markdown

````md
<details open>

<summary>Tips for collapsed sections</summary>

### You can add a header

You can add text within a collapsed section.

You can add an image or a code block, too.

    ```ruby
    puts "Hello World"
    ```

</details>
````

### HTML Parseado

```html
<details open>
  <summary>Tips for collapsed sections</summary>

  <h3>You can add a header</h3>

  <p>You can add text within a collapsed section.</p>

  <p>You can add an image or a code block, too.</p>

  <pre><code class="language-ruby">
puts "Hello World"
</code></pre>
</details>
```

## Regras Principais

* `<details>` e `<summary>` sao **raw HTML**, nao sintaxe Markdown
* Markdown dentro de `<details>` **ainda e parseado**
* Syntax highlighting funciona normalmente dentro de secoes colapsadas
* Use `<summary>` como **rotulo clicavel**

## Paragrafos com HTML Inline e SVG

### Markdown

```md
You can streamline your Markdown by creating a collapsed section with the `<details>` tag.
```

### HTML Parseado

```html
<p>
  You can streamline your Markdown by creating a collapsed section with the <code>&lt;details&gt;</code> tag.
</p>
```

---

### Markdown (inline SVG preserved)

```md
Any Markdown within the `<details>` block will be collapsed until the reader clicks <svg ...></svg> to expand the details.
```

### HTML Parseado

```html
<p>
  Any Markdown within the <code>&lt;details&gt;</code> block will be collapsed until the reader clicks
  <svg version="1.1" width="16" height="16" viewBox="0 0 16 16"
       class="octicon octicon-triangle-right"
       aria-label="The right triangle icon"
       role="img">
    <path d="m6.427 4.427 3.396 3.396a.25.25 0 0 1 0 .354l-3.396 3.396A.25.25 0 0 1 6 11.396V4.604a.25.25 0 0 1 .427-.177Z"></path>
  </svg>
  to expand the details.
</p>
```
