---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Garanta que tipos C# sejam documentados com comentarios XML e sigam boas praticas de documentacao.'
---

# Boas Praticas de Documentacao em C#

- Membros publicos devem ser documentados com comentarios XML.
- E recomendado documentar membros internos, especialmente se forem complexos ou nao autoexplicativos.

## Orientacoes para todas as APIs

- Use `<summary>` para fornecer uma descricao breve, de uma frase, do que o tipo ou membro faz. Comece o summary com um verbo no presente, na terceira pessoa.
- Use `<remarks>` para informacoes adicionais, que podem incluir detalhes de implementacao, notas de uso ou contexto relevante.
- Use `<see langword>` para palavras-chave da linguagem como `null`, `true`, `false`, `int`, `bool`, etc.
- Use `<c>` para snippets de codigo inline.
- Use `<example>` para exemplos de uso de como utilizar o membro.
  - Use `<code>` para blocos de codigo. Tags `<code>` devem ser colocadas dentro de `<example>`. Adicione a linguagem do exemplo com o atributo `language`, por exemplo, `<code language="csharp">`.
- Use `<see cref>` para referenciar outros tipos ou membros inline (em uma frase).
- Use `<seealso>` para referencias standalone (nao em frase) a outros tipos ou membros na secao "See also" da documentacao.
- Use `<inheritdoc/>` para herdar documentacao de classes base ou interfaces.
  - A menos que haja mudanca de comportamento relevante, caso em que voce deve documentar as diferencas.

## Metodos

- Use `<param>` para descrever parametros de metodos.
  - A descricao deve ser uma frase nominal que nao especifique o tipo de dado.
  - Comece com um artigo introdutorio.
  - Se o parametro for um flag enum, inicie com "A bitwise combination of the enumeration values that specifies...".
  - Se o parametro for um non-flag enum, inicie com "One of the enumeration values that specifies...".
  - Se o parametro for Boolean, use o formato "`<see langword="true" />` to ...; otherwise, `<see langword="false" />`.".
  - Se o parametro for "out", use o formato "When this method returns, contains .... This parameter is treated as uninitialized.".
- Use `<paramref>` para referenciar nomes de parametros na documentacao.
- Use `<typeparam>` para descrever parametros de tipo em generics.
- Use `<typeparamref>` para referenciar parametros de tipo na documentacao.
- Use `<returns>` para descrever o retorno do metodo.
  - A descricao deve ser uma frase nominal que nao especifique o tipo de dado.
  - Comece com um artigo introdutorio.
  - Se o retorno for Boolean, use o formato "`<see langword="true" />` if ...; otherwise, `<see langword="false" />`.".

## Construtores

- O summary deve ser "Initializes a new instance of the <Class> class [or struct].".

## Propriedades

- O `<summary>` deve iniciar com:
  - "Gets or sets..." para propriedade de leitura/escrita.
  - "Gets..." para propriedade somente leitura.
  - "Gets [or sets] a value that indicates whether..." para propriedades Boolean.
- Use `<value>` para descrever o valor da propriedade.
  - A descricao deve ser uma frase nominal que nao especifique o tipo de dado.
  - Se a propriedade tiver valor default, adicione em uma frase separada, por exemplo, "The default is `<see langword="false" />`".
  - Se o tipo for Boolean, use o formato "`<see langword="true" />` if ...; otherwise, `<see langword="false" />`. The default is ...".

## Excecoes

- Use `<exception cref>` para documentar excecoes lancadas por construtores, propriedades, indexers, metodos, operadores e eventos.
- Documente todas as excecoes lancadas diretamente pelo membro.
- Para excecoes lancadas por membros internos, documente apenas as excecoes que usuarios provavelmente encontrarao.
- A descricao da excecao deve explicar a condicao em que ela e lancada.
  - Omitir "Thrown if ..." ou "If ..." no inicio da frase. Apenas descreva a condicao diretamente, por exemplo "An error occurred when accessing a Message Queuing API.".
