# Organizing information with collapsed sections

Voce pode simplificar seu Markdown criando uma secao colapsada com a tag `<details>`.

## Creating a collapsed section

Voce pode ocultar temporariamente secoes do seu Markdown criando uma secao colapsada que o leitor pode escolher expandir. Por exemplo, quando voce quer incluir detalhes tecnicos em um comentario de issue que podem nao ser relevantes ou interessantes para todos, voce pode colocar esses detalhes em uma secao colapsada.

Qualquer Markdown dentro do bloco `<details>` ficara colapsado ate o leitor clicar no <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-triangle-right" aria-label="The right triangle icon" role="img"><path d="m6.427 4.427 3.396 3.396a.25.25 0 0 1 0 .354l-3.396 3.396A.25.25 0 0 1 6 11.396V4.604a.25.25 0 0 1 .427-.177Z"></path></svg> para expandir os detalhes.

Dentro do bloco `<details>`, use a tag `<summary>` para avisar aos leitores o que ha dentro. O rotulo aparece a direita de <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-triangle-right" aria-label="The right triangle icon" role="img"><path d="m6.427 4.427 3.396 3.396a.25.25 0 0 1 0 .354l-3.396 3.396A.25.25 0 0 1 6 11.396V4.604a.25.25 0 0 1 .427-.177Z"></path></svg>.

````markdown
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

O Markdown dentro do rotulo `<summary>` ficara colapsado por default:

![Screenshot do Markdown acima nesta pagina conforme renderizado no GitHub, mostrando uma seta apontando para a direita e o titulo "Dicas para secoes colapsadas.".](https://docs.github.com/assets/images/help/writing/collapsed-section-view.png)

Depois que o leitor clica no <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-triangle-right" aria-label="The right triangle icon" role="img"><path d="m6.427 4.427 3.396 3.396a.25.25 0 0 1 0 .354l-3.396 3.396A.25.25 0 0 1 6 11.396V4.604a.25.25 0 0 1 .427-.177Z"></path></svg>, os detalhes sao expandidos:

![Screenshot do Markdown acima nesta pagina conforme renderizado no GitHub. A secao colapsada contem headers, texto, imagens e code blocks.](https://docs.github.com/assets/images/help/writing/open-collapsed-section.png)

Opcionalmente, para deixar a secao aberta por default, adicione o atributo `open` na tag `<details>`:

```html
<details open>
```

## Further reading

* [GitHub Flavored Markdown Spec](https://github.github.com/gfm/)
* [Basic writing and formatting syntax](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
