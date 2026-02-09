---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
description: 'Boas praticas para testes unitarios com JUnit 5, incluindo testes data-driven.'
---

# Boas Praticas de JUnit 5+

Seu objetivo e me ajudar a escrever testes unitarios efetivos com JUnit 5, cobrindo abordagens padrao e data-driven.

## Setup do Projeto

- Use uma estrutura padrao de projeto Maven ou Gradle.
- Coloque o codigo de teste em `src/test/java`.
- Inclua dependencias para `junit-jupiter-api`, `junit-jupiter-engine` e `junit-jupiter-params` para testes parametrizados.
- Use comandos da ferramenta de build para rodar testes: `mvn test` ou `gradle test`.

## Estrutura de Teste

- Classes de teste devem ter o sufixo `Test`, por exemplo, `CalculatorTest` para a classe `Calculator`.
- Use `@Test` para metodos de teste.
- Siga o padrao Arrange-Act-Assert (AAA).
- Nomeie testes usando uma convencao descritiva, como `methodName_should_expectedBehavior_when_scenario`.
- Use `@BeforeEach` e `@AfterEach` para setup e teardown por teste.
- Use `@BeforeAll` e `@AfterAll` para setup e teardown por classe (devem ser metodos estaticos).
- Use `@DisplayName` para fornecer um nome legivel para classes e metodos de teste.

## Testes Padrao

- Mantenha os testes focados em um unico comportamento.
- Evite testar varias condicoes em um unico metodo.
- Torne os testes independentes e idempotentes (podem rodar em qualquer ordem).
- Evite interdependencia entre testes.

## Testes Data-Driven (Parametrizados)

- Use `@ParameterizedTest` para marcar um metodo como parametrizado.
- Use `@ValueSource` para valores literais simples (strings, ints, etc.).
- Use `@MethodSource` para referenciar um metodo factory que fornece argumentos como `Stream`, `Collection`, etc.
- Use `@CsvSource` para valores inline separados por virgula.
- Use `@CsvFileSource` para usar um arquivo CSV do classpath.
- Use `@EnumSource` para usar constantes de enum.

## Assertivas

- Use os metodos estaticos de `org.junit.jupiter.api.Assertions` (por exemplo, `assertEquals`, `assertTrue`, `assertNotNull`).
- Para assertivas mais fluent e legiveis, considere usar uma biblioteca como AssertJ (`assertThat(...).is...`).
- Use `assertThrows` ou `assertDoesNotThrow` para testar excecoes.
- Agrupe assertivas relacionadas com `assertAll` para garantir que todas sejam verificadas antes do teste falhar.
- Use mensagens descritivas nas assertivas para dar clareza em caso de falha.

## Mocking e Isolamento

- Use um framework de mocking como Mockito para criar objetos mock de dependencias.
- Use anotacoes `@Mock` e `@InjectMocks` do Mockito para simplificar criacao e injecao de mocks.
- Use interfaces para facilitar mocking.

## Organizacao de Testes

- Agrupe testes por feature ou componente usando packages.
- Use `@Tag` para categorizar testes (por exemplo, `@Tag("fast")`, `@Tag("integration")`).
- Use `@TestMethodOrder(MethodOrderer.OrderAnnotation.class)` e `@Order` para controlar a ordem de execucao quando estritamente necessario.
- Use `@Disabled` para pular temporariamente um metodo ou classe de teste, fornecendo um motivo.
- Use `@Nested` para agrupar testes em uma classe interna aninhada e melhorar organizacao e estrutura.
