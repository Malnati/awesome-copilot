# Markdown Basico para HTML

## Titulos

### Markdown

```md
# Basic writing and formatting syntax
```

### HTML Parseado

```html
<h1>Basic writing and formatting syntax</h1>
```

```md
## Headings
```

```html
<h2>Headings</h2>
```

```md
### A third-level heading
```

```html
<h3>A third-level heading</h3>
```

### Markdown

```md
Heading 2
---
```

### HTML Parseado

```html
<h2>Heading 2</h2>
```

---

## Paragrafos

### Markdown

```md
Create sophisticated formatting for your prose and code on GitHub with simple syntax.
```

### HTML Parseado

```html
<p>Create sophisticated formatting for your prose and code on GitHub with simple syntax.</p>
```

---

## Formatacao Inline

### Negrito

```md
**This is bold text**
```

```html
<strong>This is bold text</strong>
```

---

### Italico

```md
_This text is italicized_
```

```html
<em>This text is italicized</em>
```

---

### Negrito + Italico

```md
***All this text is important***
```

```html
<strong><em>All this text is important</em></strong>
```

---

### Tachado (GFM)

```md
~~This was mistaken text~~
```

```html
<del>This was mistaken text</del>
```

---

### Subscript / Superscript (raw HTML passthrough)

```md
This is a <sub>subscript</sub> text
```

```html
<p>This is a <sub>subscript</sub> text</p>
```

```md
This is a <sup>superscript</sup> text
```

```html
<p>This is a <sup>superscript</sup> text</p>
```

---

## Blockquotes

### Markdown

```md
> Text that is a quote
```

### HTML Parseado

```html
<blockquote>
  <p>Text that is a quote</p>
</blockquote>
```

---

### GitHub Alert (NOTE)

```md
> [!NOTE]
> Useful information.
```

```html
<blockquote class="markdown-alert markdown-alert-note">
  <p><strong>Note</strong></p>
  <p>Useful information.</p>
</blockquote>
```

> ⚠️ As classes `markdown-alert-*` sao especificas do GitHub, nao padrao Markdown.

---

## Inline Code

```md
Use `git status` to list files.
```

```html
<p>Use <code>git status</code> to list files.</p>
```

---

## Code Blocks

### Markdown

````md
```markdown
git status
git add
```
````

### HTML Parseado

```html
<pre><code class="language-markdown">
git status
git add
</code></pre>
```

---

## Tabelas

### Markdown

```md
| Style | Syntax |
|------|--------|
| Bold | ** ** |
```

### HTML Parseado

```html
<table>
  <thead>
    <tr>
      <th>Style</th>
      <th>Syntax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Bold</td>
      <td><strong> </strong></td>
    </tr>
  </tbody>
</table>
```

---

## Links

### Markdown

```md
[GitHub Pages](https://pages.github.com/)
```

### HTML Parseado

```html
<a href="https://pages.github.com/">GitHub Pages</a>
```

---

## Imagens

### Markdown

```md
![Alt text](image.png)
```

### HTML Parseado

```html
<img src="image.png" alt="Alt text">
```

---

## Listas

### Lista Nao Ordenada

```md
- George Washington
- John Adams
```

```html
<ul>
  <li>George Washington</li>
  <li>John Adams</li>
</ul>
```

---

### Lista Ordenada

```md
1. James Madison
2. James Monroe
```

```html
<ol>
  <li>James Madison</li>
  <li>James Monroe</li>
</ol>
```

---

### Listas Aninhadas

```md
1. First item
   - Nested item
```

```html
<ol>
  <li>
    First item
    <ul>
      <li>Nested item</li>
    </ul>
  </li>
</ol>
```

---

## Task Lists (GitHub Flavored Markdown)

```md
- [x] Done
- [ ] Pending
```

```html
<ul>
  <li>
    <input type="checkbox" checked disabled> Done
  </li>
  <li>
    <input type="checkbox" disabled> Pending
  </li>
</ul>
```

---

## Mentions

```md
@github/support
```

```html
<a href="https://github.com/github/support" class="user-mention">@github/support</a>
```

---

## Footnotes

### Markdown

```md
Here is a footnote[^1].

[^1]: My reference.
```

### HTML Parseado

```html
<p>
  Here is a footnote
  <sup id="fnref-1">
    <a href="#fn-1">1</a>
  </sup>.
</p>

<section class="footnotes">
  <ol>
    <li id="fn-1">
      <p>My reference.</p>
    </li>
  </ol>
</section>
```

---

## HTML Comments (Hidden Content)

```md
<!-- This content will not appear -->
```

```html
<!-- This content will not appear -->
```

---

## Escaped Markdown Characters

```md
\*not italic\*
```

```html
<p>*not italic*</p>
```

---

## Emoji

```md
:+1:
```

```html
<img class="emoji" alt="👍" src="...">
```

(GitHub replaces emoji with `<img>` tags.)

---
