---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
description: 'Obtenha boas praticas para testes unitarios MSTest 3.x/4.x, incluindo APIs modernas de assertion e testes data-driven'
---

# Boas Praticas de MSTest (MSTest 3.x/4.x)

Seu objetivo e me ajudar a escrever testes unitarios eficazes com MSTest moderno, usando APIs atuais e boas praticas.

## Setup do Projeto

- Use um projeto de testes separado com convencao `[ProjectName].Tests`
- Referencie pacotes NuGet MSTest 3.x+ (inclui analyzers)
- Considere usar MSTest.Sdk para simplificar o setup
- Rode testes com `dotnet test`

## Estrutura de Classe de Teste

- Use `[TestClass]` em classes de teste
- **Seal test classes por default** para performance e clareza de design
- Use `[TestMethod]` para metodos de teste (prefira em vez de `[DataTestMethod]`)
- Siga o pattern Arrange-Act-Assert (AAA)
- Nomeie testes com o pattern `MethodName_Scenario_ExpectedBehavior`

```csharp
[TestClass]
public sealed class CalculatorTests
{
    [TestMethod]
    public void Add_TwoPositiveNumbers_ReturnsSum()
    {
        // Arrange
        var calculator = new Calculator();

        // Act
        var result = calculator.Add(2, 3);

        // Assert
        Assert.AreEqual(5, result);
    }
}
```

## Ciclo de Vida de Teste

- **Prefira construtores em vez de `[TestInitialize]`** - habilita campos `readonly` e segue patterns C#
- Use `[TestCleanup]` para cleanup que deve rodar mesmo se o teste falhar
- Combine construtor com `[TestInitialize]` async quando precisar de setup async

```csharp
[TestClass]
public sealed class ServiceTests
{
    private readonly MyService _service;  // readonly habilitado via construtor

    public ServiceTests()
    {
        _service = new MyService();
    }

    [TestInitialize]
    public async Task InitAsync()
    {
        // Use apenas para inicializacao async
        await _service.WarmupAsync();
    }

    [TestCleanup]
    public void Cleanup() => _service.Reset();
}
```

### Ordem de Execucao

1. **Assembly Initialization** - `[AssemblyInitialize]` (uma vez por assembly)
2. **Class Initialization** - `[ClassInitialize]` (uma vez por classe)
3. **Test Initialization** (para cada metodo):
   1. Construtor
   2. Set `TestContext` property
   3. `[TestInitialize]`
4. **Test Execution** - metodo de teste roda
5. **Test Cleanup** (para cada metodo):
   1. `[TestCleanup]`
   2. `DisposeAsync` (se implementado)
   3. `Dispose` (se implementado)
6. **Class Cleanup** - `[ClassCleanup]` (uma vez por classe)
7. **Assembly Cleanup** - `[AssemblyCleanup]` (uma vez por assembly)

## APIs Modernas de Assertion

MSTest fornece tres classes de assertion: `Assert`, `StringAssert` e `CollectionAssert`.

### Assert Class - Assertions Core

```csharp
// Equality
Assert.AreEqual(expected, actual);
Assert.AreNotEqual(notExpected, actual);
Assert.AreSame(expectedObject, actualObject);      // Reference equality
Assert.AreNotSame(notExpectedObject, actualObject);

// Null checks
Assert.IsNull(value);
Assert.IsNotNull(value);

// Boolean
Assert.IsTrue(condition);
Assert.IsFalse(condition);

// Fail/Inconclusive
Assert.Fail("Test failed due to...");
Assert.Inconclusive("Test cannot be completed because...");
```

### Teste de Excecao (Prefira a `[ExpectedException]`)

```csharp
// Assert.Throws - matches TException or derived types
var ex = Assert.Throws<ArgumentException>(() => Method(null));
Assert.AreEqual("Value cannot be null.", ex.Message);

// Assert.ThrowsExactly - matches exact type only
var ex = Assert.ThrowsExactly<InvalidOperationException>(() => Method());

// Async versions
var ex = await Assert.ThrowsAsync<HttpRequestException>(async () => await client.GetAsync(url));
var ex = await Assert.ThrowsExactlyAsync<InvalidOperationException>(async () => await Method());
```

### Collection Assertions (Assert class)

```csharp
Assert.Contains(expectedItem, collection);
Assert.DoesNotContain(unexpectedItem, collection);
Assert.ContainsSingle(collection);  // exactly one element
Assert.HasCount(5, collection);
Assert.IsEmpty(collection);
Assert.IsNotEmpty(collection);
```

### String Assertions (Assert class)

```csharp
Assert.Contains("expected", actualString);
Assert.StartsWith("prefix", actualString);
Assert.EndsWith("suffix", actualString);
Assert.DoesNotStartWith("prefix", actualString);
Assert.DoesNotEndWith("suffix", actualString);
Assert.MatchesRegex(@"\d{3}-\d{4}", phoneNumber);
Assert.DoesNotMatchRegex(@"\d+", textOnly);
```

### Comparison Assertions

```csharp
Assert.IsGreaterThan(lowerBound, actual);
Assert.IsGreaterThanOrEqualTo(lowerBound, actual);
Assert.IsLessThan(upperBound, actual);
Assert.IsLessThanOrEqualTo(upperBound, actual);
Assert.IsInRange(actual, low, high);
Assert.IsPositive(number);
Assert.IsNegative(number);
```

### Type Assertions

```csharp
// MSTest 3.x - uses out parameter
Assert.IsInstanceOfType<MyClass>(obj, out var typed);
typed.DoSomething();

// MSTest 4.x - returns typed result directly
var typed = Assert.IsInstanceOfType<MyClass>(obj);
typed.DoSomething();

Assert.IsNotInstanceOfType<WrongType>(obj);
```

### Assert.That (MSTest 4.0+)

```csharp
Assert.That(result.Count > 0);  // Auto-captures expression in failure message
```

### StringAssert Class

> **Note:** Prefira equivalentes em `Assert` quando disponiveis (ex.: `Assert.Contains("expected", actual)` em vez de `StringAssert.Contains(actual, "expected")`).

```csharp
StringAssert.Contains(actualString, "expected");
StringAssert.StartsWith(actualString, "prefix");
StringAssert.EndsWith(actualString, "suffix");
StringAssert.Matches(actualString, new Regex(@"\d{3}-\d{4}"));
StringAssert.DoesNotMatch(actualString, new Regex(@"\d+"));
```

### CollectionAssert Class

> **Note:** Prefira equivalentes em `Assert` quando disponiveis (ex.: `Assert.Contains`).

```csharp
// Containment
CollectionAssert.Contains(collection, expectedItem);
CollectionAssert.DoesNotContain(collection, unexpectedItem);

// Equality (same elements, same order)
CollectionAssert.AreEqual(expectedCollection, actualCollection);
CollectionAssert.AreNotEqual(unexpectedCollection, actualCollection);

// Equivalence (same elements, any order)
CollectionAssert.AreEquivalent(expectedCollection, actualCollection);
CollectionAssert.AreNotEquivalent(unexpectedCollection, actualCollection);

// Subset checks
CollectionAssert.IsSubsetOf(subset, superset);
CollectionAssert.IsNotSubsetOf(notSubset, collection);

// Element validation
CollectionAssert.AllItemsAreInstancesOfType(collection, typeof(MyClass));
CollectionAssert.AllItemsAreNotNull(collection);
CollectionAssert.AllItemsAreUnique(collection);
```

## Testes Data-Driven

### DataRow

```csharp
[TestMethod]
[DataRow(1, 2, 3)]
[DataRow(0, 0, 0, DisplayName = "Zeros")]
[DataRow(-1, 1, 0, IgnoreMessage = "Known issue #123")]  // MSTest 3.8+
public void Add_ReturnsSum(int a, int b, int expected)
{
    Assert.AreEqual(expected, Calculator.Add(a, b));
}
```

### DynamicData

A fonte de dados pode retornar:

- `IEnumerable<(T1, T2, ...)>` (ValueTuple) - **preferred**, oferece type safety (MSTest 3.7+)
- `IEnumerable<Tuple<T1, T2, ...>>` - oferece type safety
- `IEnumerable<TestDataRow>` - oferece type safety + controle de metadata (display name, categories)
- `IEnumerable<object[]>` - **least preferred**, sem type safety

> **Note:** Ao criar novos metodos de dados de teste, prefira `ValueTuple` ou `TestDataRow` em vez de `IEnumerable<object[]>`. `object[]` nao oferece type checking e pode causar erros em runtime.

```csharp
[TestMethod]
[DynamicData(nameof(TestData))]
public void DynamicTest(int a, int b, int expected)
{
    Assert.AreEqual(expected, Calculator.Add(a, b));
}

// ValueTuple - preferred (MSTest 3.7+)
public static IEnumerable<(int a, int b, int expected)> TestData =>
[
    (1, 2, 3),
    (0, 0, 0),
];

// TestDataRow - quando precisar de display name ou metadata
public static IEnumerable<TestDataRow<(int a, int b, int expected)>> TestDataWithMetadata =>
[
    new((1, 2, 3)) { DisplayName = "Positive numbers" },
    new((0, 0, 0)) { DisplayName = "Zeros" },
    new((-1, 1, 0)) { DisplayName = "Mixed signs", IgnoreMessage = "Known issue #123" },
];

// IEnumerable<object[]> - evitar em codigo novo (sem type safety)
public static IEnumerable<object[]> LegacyTestData =>
[
    [1, 2, 3],
    [0, 0, 0],
];
```

## TestContext

A classe `TestContext` fornece informacoes do test run, suporte a cancelamento e metodos de output.
Consulte [TestContext documentation](https://learn.microsoft.com/dotnet/core/testing/unit-testing-mstest-writing-tests-testcontext) para referencia completa.

### Acessando TestContext

```csharp
// Property (MSTest suprime CS8618 - nao use nullable ou = null!)
public TestContext TestContext { get; set; }

// Constructor injection (MSTest 3.6+) - preferido para imutabilidade
[TestClass]
public sealed class MyTests
{
    private readonly TestContext _testContext;

    public MyTests(TestContext testContext)
    {
        _testContext = testContext;
    }
}

// Metodos estaticos recebem como parametro
[ClassInitialize]
public static void ClassInit(TestContext context) { }

// Opcional para metodos de cleanup (MSTest 3.6+)
[ClassCleanup]
public static void ClassCleanup(TestContext context) { }

[AssemblyCleanup]
public static void AssemblyCleanup(TestContext context) { }
```

### Cancellation Token

Sempre use `TestContext.CancellationToken` para cancelamento cooperativo com `[Timeout]`:

```csharp
[TestMethod]
[Timeout(5000)]
public async Task LongRunningTest()
{
    await _httpClient.GetAsync(url, TestContext.CancellationToken);
}
```

### Propriedades do Test Run

```csharp
TestContext.TestName              // Current test method name
TestContext.TestDisplayName       // Display name (3.7+)
TestContext.CurrentTestOutcome    // Pass/Fail/InProgress
TestContext.TestData              // Parameterized test data (3.7+, in TestInitialize/Cleanup)
TestContext.TestException         // Exception if test failed (3.7+, in TestCleanup)
TestContext.DeploymentDirectory   // Directory with deployment items
```

### Output e Arquivos de Resultado

```csharp
// Write to test output (useful for debugging)
TestContext.WriteLine("Processing item {0}", itemId);

// Attach files to test results (logs, screenshots)
TestContext.AddResultFile(screenshotPath);

// Store/retrieve data across test methods
TestContext.Properties["SharedKey"] = computedValue;
```

## Recursos Avancados

### Retry para testes flaky (MSTest 3.9+)

```csharp
[TestMethod]
[Retry(3)]
public void FlakyTest() { }
```

### Execucao Condicional (MSTest 3.10+)

Pule ou rode testes com base no OS ou ambiente de CI:

```csharp
// OS-specific tests
[TestMethod]
[OSCondition(OperatingSystems.Windows)]
public void WindowsOnlyTest() { }

[TestMethod]
[OSCondition(OperatingSystems.Linux | OperatingSystems.MacOS)]
public void UnixOnlyTest() { }

[TestMethod]
[OSCondition(ConditionMode.Exclude, OperatingSystems.Windows)]
public void SkipOnWindowsTest() { }

// CI environment tests
[TestMethod]
[CICondition]  // Runs only in CI (default: ConditionMode.Include)
public void CIOnlyTest() { }

[TestMethod]
[CICondition(ConditionMode.Exclude)]  // Skips in CI, runs locally
public void LocalOnlyTest() { }
```

### Paralelizacao

```csharp
// Assembly level
[assembly: Parallelize(Workers = 4, Scope = ExecutionScope.MethodLevel)]

// Disable for specific class
[TestClass]
[DoNotParallelize]
public sealed class SequentialTests { }
```

### Rastreabilidade de Work Item (MSTest 3.8+)

Vincule testes a work items para rastreabilidade em relatórios:

```csharp
// Azure DevOps work items
[TestMethod]
[WorkItem(12345)]  // Links to work item #12345
public void Feature_Scenario_ExpectedBehavior() { }

// Multiple work items
[TestMethod]
[WorkItem(12345)]
[WorkItem(67890)]
public void Feature_CoversMultipleRequirements() { }

// GitHub issues (MSTest 3.8+)
[TestMethod]
[GitHubWorkItem("https://github.com/owner/repo/issues/42")]
public void BugFix_Issue42_IsResolved() { }
```

Work item associations aparecem nos resultados de teste e podem ser usadas para:
- Rastrear cobertura de testes com requisitos
- Vincular correcoes de bug a testes de regressao
- Gerar relatorios de rastreabilidade em pipelines CI/CD

## Erros Comuns a Evitar

```csharp
// ❌ Wrong argument order
Assert.AreEqual(actual, expected);
// ✅ Correct
Assert.AreEqual(expected, actual);

// ❌ Using ExpectedException (obsolete)
[ExpectedException(typeof(ArgumentException))]
// ✅ Use Assert.Throws
Assert.Throws<ArgumentException>(() => Method());

// ❌ Using LINQ Single() - unclear exception
var item = items.Single();
// ✅ Use ContainsSingle - better failure message
var item = Assert.ContainsSingle(items);

// ❌ Hard cast - unclear exception
var handler = (MyHandler)result;
// ✅ Type assertion - shows actual type on failure
var handler = Assert.IsInstanceOfType<MyHandler>(result);

// ❌ Ignoring cancellation token
await client.GetAsync(url, CancellationToken.None);
// ✅ Flow test cancellation
await client.GetAsync(url, TestContext.CancellationToken);

// ❌ Making TestContext nullable - leads to unnecessary null checks
public TestContext? TestContext { get; set; }
// ❌ Using null! - MSTest already suppresses CS8618 for this property
public TestContext TestContext { get; set; } = null!;
// ✅ Declare without nullable or initializer - MSTest handles the warning
public TestContext TestContext { get; set; }
```

## Organizacao de Testes

- Agrupe testes por feature ou componente
- Use `[TestCategory("Category")]` para filtragem
- Use `[TestProperty("Name", "Value")]` para metadata custom (ex.: `[TestProperty("Bug", "12345")]`)
- Use `[Priority(1)]` para testes criticos
- Habilite analyzers relevantes do MSTest (MSTEST0020 para preferencia de construtor)

## Mocking e Isolamento

- Use Moq ou NSubstitute para mocking de dependencias
- Use interfaces para facilitar mocking
- Mocke dependencias para isolar unidades sob teste
