# Writing mathematical expressions

Use Markdown para exibir expressoes matematicas no GitHub.

## About writing mathematical expressions

Para permitir comunicacao clara de expressoes matematicas, o GitHub suporta matematica em LaTeX dentro do Markdown. Para mais informacoes, veja [LaTeX/Mathematics](http://en.wikibooks.org/wiki/LaTeX/Mathematics) no Wikibooks.

A capacidade de renderizacao matematica do GitHub usa MathJax, um mecanismo open source baseado em JavaScript. O MathJax suporta uma ampla gama de macros LaTeX e varias extensoes de acessibilidade. Para mais informacoes, veja [the MathJax documentation](http://docs.mathjax.org/en/latest/input/tex/index.html#tex-and-latex-support) e [the MathJax Accessibility Extensions Documentation](https://mathjax.github.io/MathJax-a11y/docs/#reader-guide).

A renderizacao de expressoes matematicas esta disponivel em GitHub Issues, GitHub Discussions, pull requests, wikis e arquivos Markdown.

## Writing inline expressions

Existem duas opcoes para delimitar uma expressao matematica inline. Voce pode cercar a expressao com simbolos de dolar (`$`), ou iniciar com <code>$\`</code> e terminar com <code>\`$</code>. A segunda sintaxe e util quando a expressao tem caracteres que conflitam com a sintaxe Markdown. Para mais informacoes, veja [Basic writing and formatting syntax](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

```text
This sentence uses `$` delimiters to show math inline: $\sqrt{3x-1}+(1+x)^2$
```

![Screenshot do Markdown renderizado mostrando uma expressao matematica inline: a raiz quadrada de 3x menos 1 mais (1 mais x) ao quadrado.](https://docs.github.com/assets/images/help/writing/inline-math-markdown-rendering.png)

```text
This sentence uses $\` and \`$ delimiters to show math inline: $`\sqrt{3x-1}+(1+x)^2`$
```

![Screenshot do Markdown renderizado mostrando uma expressao matematica inline com sintaxe de backtick: a raiz quadrada de 3x menos 1 mais (1 mais x) ao quadrado.](https://docs.github.com/assets/images/help/writing/inline-backtick-math-markdown-rendering.png)

## Writing expressions as blocks

Para adicionar uma expressao matematica como bloco, inicie uma nova linha e delimite a expressao com dois simbolos de dolar `$$`.

>  [!TIP] Se voce estiver escrevendo em um arquivo .md, sera necessario usar uma formatacao especifica para criar uma quebra de linha, como finalizar a linha com uma barra invertida, como no exemplo abaixo. Para mais informacoes sobre quebras de linha em Markdown, veja [Basic writing and formatting syntax](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#line-breaks).

```text
**The Cauchy-Schwarz Inequality**\
$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$
```

![Screenshot do Markdown renderizado mostrando uma equacao complexa. O texto em negrito "The Cauchy-Schwarz Inequality" aparece acima da formula da desigualdade.](https://docs.github.com/assets/images/help/writing/math-expression-as-a-block-rendering.png)

Como alternativa, voce pode usar a sintaxe de bloco de codigo <code>```math</code> para exibir uma expressao matematica como bloco. Com essa sintaxe, voce nao precisa usar delimitadores `$$`. O exemplo abaixo renderiza igual ao anterior:

````text
**The Cauchy-Schwarz Inequality**

```math
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
```
````

## Writing dollar signs in line with and within mathematical expressions

Para exibir o simbolo de dolar como caractere na mesma linha de uma expressao matematica, voce precisa escapar o `$` nao delimitador para garantir que a linha renderize corretamente.

* Dentro de uma expressao matematica, adicione um `\` antes do `$` explicito.

  ```text
  This expression uses `\$` to display a dollar sign: $`\sqrt{\$4}`$
  ```

  ![Screenshot do Markdown renderizado mostrando como uma barra invertida antes do dolar exibe o simbolo como parte de uma expressao matematica.](https://docs.github.com/assets/images/help/writing/dollar-sign-within-math-expression.png)

* Fora de uma expressao matematica, mas na mesma linha, use tags span ao redor do `$` explicito.

  ```text
  To split <span>$</span>100 in half, we calculate $100/2$
  ```

  ![Screenshot do Markdown renderizado mostrando como tags span ao redor do dolar exibem o simbolo como texto inline e nao como parte de uma equacao.](https://docs.github.com/assets/images/help/writing/dollar-sign-inline-math-expression.png)

## Further reading

* [The MathJax website](http://mathjax.org)
* [Getting started with writing and formatting on GitHub](https://docs.github.com/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github)
* [GitHub Flavored Markdown Spec](https://github.github.com/gfm/)
