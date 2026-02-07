---
agent: 'agent'
description: 'Garanta que codigo .NET/C# siga boas praticas do solution/projeto.'
---
# Boas Praticas .NET/C#

Sua tarefa e garantir que o codigo .NET/C# em ${selection} siga as boas praticas especificas deste solution/projeto. Isso inclui:

## Documentacao e Estrutura

- Crie comentarios XML abrangentes para todas as classes, interfaces, metodos e propriedades publicas
- Inclua descricoes de parametros e retorno nos comentarios XML
- Siga a estrutura de namespace estabelecida: {Core|Console|App|Service}.{Feature}

## Design Patterns e Arquitetura

- Use sintaxe de primary constructor para dependency injection (ex.: `public class MyClass(IDependency dependency)`)
- Implemente o Command Handler pattern com classes base genericas (ex.: `CommandHandler<TOptions>`)
- Use interface segregation com convencoes claras (prefixe interfaces com 'I')
- Siga o Factory pattern para criacao complexa de objetos

## Dependency Injection e Services

- Use dependency injection via construtor com null checks usando ArgumentNullException
- Registre services com lifetimes apropriados (Singleton, Scoped, Transient)
- Use patterns do Microsoft.Extensions.DependencyInjection
- Implemente interfaces de service para testabilidade

## Resource Management e Localizacao

- Use ResourceManager para mensagens localizadas e strings de erro
- Separe arquivos de resources LogMessages e ErrorMessages
- Acesse resources via `_resourceManager.GetString("MessageKey")`

## Patterns de Async/Await

- Use async/await para todas as operacoes de I/O e tasks longas
- Retorne Task ou Task<T> de metodos async
- Use ConfigureAwait(false) quando apropriado
- Trate excecoes async adequadamente

## Padroes de Teste

- Use framework MSTest com FluentAssertions para assertions
- Siga o pattern AAA (Arrange, Act, Assert)
- Use Moq para mocking de dependencias
- Teste cenarios de sucesso e falha
- Inclua testes de validacao de parametros nulos

## Configuracao e Settings

- Use classes de configuracao tipadas com data annotations
- Implemente atributos de validacao (Required, NotEmptyOrWhitespace)
- Use IConfiguration binding para settings
- Suporte arquivos appsettings.json

## Semantic Kernel e Integracao de IA

- Use Microsoft.SemanticKernel para operacoes de IA
- Implemente configuracao adequada do kernel e registro de services
- Trate settings de modelos de IA (ChatCompletion, Embedding, etc.)
- Use patterns de output estruturado para respostas confiaveis

## Tratamento de Erros e Logging

- Use logging estruturado com Microsoft.Extensions.Logging
- Inclua scoped logging com contexto significativo
- Lance excecoes especificas com mensagens descritivas
- Use try-catch para cenarios de falha esperados

## Performance e Seguranca

- Use features de C# 12+ e otimizacoes .NET 8 quando aplicavel
- Implemente validacao e sanitizacao adequada de input
- Use queries parametrizadas para operacoes de banco
- Siga praticas de codigo seguro para operacoes de IA/ML

## Qualidade de Codigo

- Garanta conformidade com SOLID
- Evite duplicacao com classes base e utilitarios
- Use nomes significativos que reflitam conceitos de dominio
- Mantenha metodos focados e coesos
- Implemente patterns de disposal adequados para resources
