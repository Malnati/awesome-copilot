---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
description: 'Obtenha boas praticas para testes unitarios com TUnit, incluindo testes data-driven'
---

# Boas Praticas de TUnit

Seu objetivo e me ajudar a escrever testes unitarios eficazes com TUnit, cobrindo abordagens padrao e data-driven.

## Setup do Projeto

- Use um projeto de testes separado com convencao `[ProjectName].Tests`
- Referencie os pacotes TUnit e TUnit.Assertions para fluent assertions
- Crie classes de teste que correspondam as classes testadas (ex.: `CalculatorTests` para `Calculator`)
- Use comandos do .NET SDK: `dotnet test` para rodar testes
- TUnit requer .NET 8.0 ou superior

## Estrutura de Testes

- Nao sao necessarios atributos em classes de teste (como xUnit/NUnit)
- Use `[Test]` em metodos de teste (nao `[Fact]` como xUnit)
- Siga o pattern Arrange-Act-Assert (AAA)
- Nomeie testes com o pattern `MethodName_Scenario_ExpectedBehavior`
- Use hooks de lifecycle: `[Before(Test)]` para setup e `[After(Test)]` para teardown
- Use `[Before(Class)]` e `[After(Class)]` para contexto compartilhado por classe
- Use `[Before(Assembly)]` e `[After(Assembly)]` para contexto compartilhado entre classes
- TUnit suporta hooks avancados como `[Before(TestSession)]` e `[After(TestSession)]`

## Testes Padrao

- Mantenha testes focados em um unico comportamento
- Evite testar multiplos comportamentos em um unico metodo
- Use syntax de assertion fluente do TUnit com `await Assert.That()`
- Inclua apenas assertions necessarias para validar o caso de teste
- Torne testes independentes e idempotentes (podem rodar em qualquer ordem)
- Evite dependencias entre testes (use `[DependsOn]` se necessario)

## Testes Data-Driven

- Use `[Arguments]` para dados inline (equivalente ao `[InlineData]` do xUnit)
- Use `[MethodData]` para dados baseados em metodo (equivalente ao `[MemberData]`)
- Use `[ClassData]` para dados baseados em classe
- Crie fontes custom de dados implementando `ITestDataSource`
- Use nomes de parametros significativos em testes data-driven
- Multiplos `[Arguments]` podem ser aplicados ao mesmo metodo

## Assertions

- Use `await Assert.That(value).IsEqualTo(expected)` para igualdade de valor
- Use `await Assert.That(value).IsSameReferenceAs(expected)` para igualdade de referencia
- Use `await Assert.That(value).IsTrue()` ou `await Assert.That(value).IsFalse()` para booleanos
- Use `await Assert.That(collection).Contains(item)` ou `await Assert.That(collection).DoesNotContain(item)` para colecoes
- Use `await Assert.That(value).Matches(pattern)` para regex
- Use `await Assert.That(action).Throws<TException>()` ou `await Assert.That(asyncAction).ThrowsAsync<TException>()` para excecoes
- Encadeie assertions com `.And`: `await Assert.That(value).IsNotNull().And.IsEqualTo(expected)`
- Use `.Or` para condicoes alternativas: `await Assert.That(value).IsEqualTo(1).Or.IsEqualTo(2)`
- Use `.Within(tolerance)` para comparacoes com tolerancia
- Todas as assertions sao assincronas e devem ser aguardadas

## Recursos Avancados

- Use `[Repeat(n)]` para repetir testes
- Use `[Retry(n)]` para retry automatico em falha
- Use `[ParallelLimit<T>]` para controlar limites de execucao paralela
- Use `[Skip("reason")]` para pular testes condicionalmente
- Use `[DependsOn(nameof(OtherTest))]` para criar dependencias
- Use `[Timeout(milliseconds)]` para timeouts
- Crie atributos custom estendendo atributos base do TUnit

## Organizacao de Testes

- Agrupe testes por feature ou componente
- Use `[Category("CategoryName")]` para categorizacao
- Use `[DisplayName("Custom Test Name")]` para nomes custom
- Considere usar `TestContext` para diagnosticos
- Use atributos condicionais como `[WindowsOnly]` para testes especificos por plataforma

## Performance e Execucao Paralela

- TUnit executa testes em paralelo por default (ao contrario de xUnit)
- Use `[NotInParallel]` para desabilitar execucao paralela em testes especificos
- Use `[ParallelLimit<T>]` com classes de limite custom para controlar concorrencia
- Testes na mesma classe rodam sequencialmente por default
- Use `[Repeat(n)]` com `[ParallelLimit<T>]` para cenarios de load testing

## Migracao a partir de xUnit

- Substitua `[Fact]` por `[Test]`
- Substitua `[Theory]` por `[Test]` e use `[Arguments]` para dados
- Substitua `[InlineData]` por `[Arguments]`
- Substitua `[MemberData]` por `[MethodData]`
- Substitua `Assert.Equal` por `await Assert.That(actual).IsEqualTo(expected)`
- Substitua `Assert.True` por `await Assert.That(condition).IsTrue()`
- Substitua `Assert.Throws<T>` por `await Assert.That(action).Throws<T>()`
- Substitua construtor/IDisposable por `[Before(Test)]`/`[After(Test)]`
- Substitua `IClassFixture<T>` por `[Before(Class)]`/`[After(Class)]`

**Por que TUnit em vez de xUnit?**

TUnit oferece uma experiencia moderna, rapida e flexivel com recursos avancados ausentes no xUnit, como assertions assincronas, hooks de lifecycle refinados e melhores capacidades de testes data-driven. As fluent assertions do TUnit sao mais claras e expressivas, sendo especialmente adequadas para projetos .NET complexos.
