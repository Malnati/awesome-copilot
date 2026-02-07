---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
description: 'Obtenha boas praticas para testes unitarios com XUnit, incluindo testes data-driven'
---

# Boas Praticas de XUnit

Seu objetivo e me ajudar a escrever testes unitarios eficazes com XUnit, cobrindo abordagens padrao e data-driven.

## Setup do Projeto

- Use um projeto de testes separado com convencao `[ProjectName].Tests`
- Referencie pacotes Microsoft.NET.Test.Sdk, xunit e xunit.runner.visualstudio
- Crie classes de teste que correspondam as classes testadas (ex.: `CalculatorTests` para `Calculator`)
- Use comandos do .NET SDK: `dotnet test` para rodar testes

## Estrutura de Testes

- Nao sao necessarios atributos em classes de teste (diferente de MSTest/NUnit)
- Use testes baseados em fatos com `[Fact]` para casos simples
- Siga o pattern Arrange-Act-Assert (AAA)
- Nomeie testes com o pattern `MethodName_Scenario_ExpectedBehavior`
- Use construtor para setup e `IDisposable.Dispose()` para teardown
- Use `IClassFixture<T>` para contexto compartilhado entre testes da classe
- Use `ICollectionFixture<T>` para contexto compartilhado entre multiplas classes de teste

## Testes Padrao

- Mantenha testes focados em um unico comportamento
- Evite testar multiplos comportamentos em um unico metodo
- Use assertions claras que expressem a intencao
- Inclua apenas assertions necessarias para validar o caso de teste
- Torne testes independentes e idempotentes (podem rodar em qualquer ordem)
- Evite dependencias entre testes

## Testes Data-Driven

- Use `[Theory]` combinado com atributos de fonte de dados
- Use `[InlineData]` para dados inline
- Use `[MemberData]` para dados baseados em metodo
- Use `[ClassData]` para dados baseados em classe
- Crie atributos custom implementando `DataAttribute`
- Use nomes de parametros significativos em testes data-driven

## Assertions

- Use `Assert.Equal` para igualdade de valor
- Use `Assert.Same` para igualdade de referencia
- Use `Assert.True`/`Assert.False` para booleanos
- Use `Assert.Contains`/`Assert.DoesNotContain` para colecoes
- Use `Assert.Matches`/`Assert.DoesNotMatch` para regex
- Use `Assert.Throws<T>` ou `await Assert.ThrowsAsync<T>` para excecoes
- Use fluent assertions para asserts mais legiveis

## Mocking e Isolamento

- Considere usar Moq ou NSubstitute junto com XUnit
- Mocke dependencias para isolar unidades sob teste
- Use interfaces para facilitar mocking
- Considere usar um DI container para setups complexos

## Organizacao de Testes

- Agrupe testes por feature ou componente
- Use `[Trait("Category", "CategoryName")]` para categorizacao
- Use collection fixtures para agrupar testes com dependencias compartilhadas
- Considere output helpers (`ITestOutputHelper`) para diagnosticos
- Pule testes condicionalmente com `Skip = "reason"` em atributos fact/theory
