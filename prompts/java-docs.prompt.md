---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Garanta que tipos Java estejam documentados com comentarios Javadoc e sigam boas praticas de documentacao.'
---

# Boas Praticas de Documentacao Java (Javadoc)

- Membros publicos e protected devem ser documentados com comentarios Javadoc.
- E recomendado documentar membros package-private e private tambem, especialmente se forem complexos ou nao autoexplicativos.
- A primeira frase do comentario Javadoc e a descricao de resumo. Deve ser uma visao concisa do que o metodo faz e terminar com ponto.
- Use `@param` para parametros de metodo. A descricao comeca com letra minuscula e nao termina com ponto.
- Use `@return` para valores de retorno do metodo.
- Use `@throws` ou `@exception` para documentar excecoes lancadas por metodos.
- Use `@see` para referencias a outros tipos ou membros.
- Use `{@inheritDoc}` para herdar documentacao de classes base ou interfaces.
  - A menos que haja mudanca de comportamento significativa, caso em que voce deve documentar as diferencas.
- Use `@param <T>` para parametros de tipo em tipos ou metodos genericos.
- Use `{@code}` para trechos de codigo inline.
- Use `<pre>{@code ... }</pre>` para blocos de codigo.
- Use `@since` para indicar quando o recurso foi introduzido (por exemplo, numero de versao).
- Use `@version` para especificar a versao do membro.
- Use `@author` para especificar o autor do codigo.
- Use `@deprecated` para marcar um membro como deprecated e fornecer uma alternativa.
