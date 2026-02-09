---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
description: 'Obtenha boas praticas para testes unitarios NUnit, incluindo testes data-driven'
---

# Boas Praticas de NUnit

Seu objetivo e me ajudar a escrever testes unitarios eficazes com NUnit, cobrindo abordagens padrao e data-driven.

## Setup do Projeto

- Use um projeto de testes separado com convencao `[ProjectName].Tests`
- Referencie pacotes Microsoft.NET.Test.Sdk, NUnit e NUnit3TestAdapter
- Crie classes de teste que correspondam as classes testadas (ex.: `CalculatorTests` para `Calculator`)
- Use comandos do .NET SDK: `dotnet test` para rodar testes

## Estrutura de Testes

- Aplique `[TestFixture]` em classes de teste
- Use `[Test]` em metodos de teste
- Siga o pattern Arrange-Act-Assert (AAA)
- Nomeie testes com o pattern `MethodName_Scenario_ExpectedBehavior`
- Use `[SetUp]` e `[TearDown]` para setup/teardown por teste
- Use `[OneTimeSetUp]` e `[OneTimeTearDown]` para setup/teardown por classe
- Use `[SetUpFixture]` para setup/teardown em nivel de assembly

## Testes Padrao

- Mantenha testes focados em um unico comportamento
- Evite testar multiplos comportamentos em um unico metodo
- Use assertions claras que expressem a intencao
- Inclua apenas assertions necessarias para validar o caso de teste
- Torne testes independentes e idempotentes (podem rodar em qualquer ordem)
- Evite dependencias entre testes

## Testes Data-Driven

- Use `[TestCase]` para dados inline
- Use `[TestCaseSource]` para dados gerados programaticamente
- Use `[Values]` para combinacoes simples de parametros
- Use `[ValueSource]` para sources baseadas em propriedade ou metodo
- Use `[Random]` para valores numericos aleatorios
- Use `[Range]` para valores numericos sequenciais
- Use `[Combinatorial]` ou `[Pairwise]` para combinar multiplos parametros

## Assertions

- Use `Assert.That` com constraint model (estilo preferido do NUnit)
- Use constraints como `Is.EqualTo`, `Is.SameAs`, `Contains.Item`
- Use `Assert.AreEqual` para igualdade simples (estilo classico)
- Use `CollectionAssert` para comparacoes de colecoes
- Use `StringAssert` para asserts especificos de string
- Use `Assert.Throws<T>` ou `Assert.ThrowsAsync<T>` para testar excecoes
- Use mensagens descritivas em assertions para clareza em falhas

## Mocking e Isolamento

- Considere usar Moq ou NSubstitute junto com NUnit
- Mocke dependencias para isolar unidades sob teste
- Use interfaces para facilitar mocking
- Considere usar um DI container para setups complexos

## Organizacao de Testes

- Agrupe testes por feature ou componente
- Use categorias com `[Category("CategoryName")]`
- Use `[Order]` para controlar ordem quando necessario
- Use `[Author("DeveloperName")]` para indicar ownership
- Use `[Description]` para informacao adicional de teste
- Considere `[Explicit]` para testes que nao devem rodar automaticamente
- Use `[Ignore("Reason")]` para pular testes temporariamente
