---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Obtenha boas praticas para programacao async em C#'
---

# Boas Praticas de Programacao Async em C#

Seu objetivo e me ajudar a seguir boas praticas para programacao assincrona em C#.

## Convencoes de Nome

- Use o sufixo "Async" para todos os metodos async
- Alinhe nomes com seus equivalentes sincronos quando aplicavel (ex.: `GetDataAsync()` para `GetData()`)

## Tipos de Retorno

- Retorne `Task<T>` quando o metodo retorna um valor
- Retorne `Task` quando o metodo nao retorna valor
- Considere `ValueTask<T>` para cenarios de alta performance e reduzir alocacoes
- Evite retornar `void` para metodos async, exceto para event handlers

## Tratamento de Excecoes

- Use try/catch ao redor de expressoes await
- Evite engolir excecoes em metodos async
- Use `ConfigureAwait(false)` quando apropriado para evitar deadlocks em code de biblioteca
- Propague excecoes com `Task.FromException()` em vez de dar throw em metodos async que retornam Task

## Performance

- Use `Task.WhenAll()` para execucao paralela de varias tasks
- Use `Task.WhenAny()` para implementar timeouts ou pegar a primeira task concluida
- Evite async/await desnecessarios quando apenas repassar resultados de tasks
- Considere cancellation tokens para operacoes longas

## Armadilhas Comuns

- Nunca use `.Wait()`, `.Result` ou `.GetAwaiter().GetResult()` em codigo async
- Evite misturar codigo bloqueante e async
- Nao crie metodos async void (exceto event handlers)
- Sempre aguarde metodos que retornam Task

## Patterns de Implementacao

- Implemente o async command pattern para operacoes longas
- Use async streams (IAsyncEnumerable<T>) para processar sequencias de forma assincrona
- Considere o task-based asynchronous pattern (TAP) para APIs publicas

Ao revisar meu codigo C#, identifique esses problemas e sugira melhorias que sigam essas boas praticas.
