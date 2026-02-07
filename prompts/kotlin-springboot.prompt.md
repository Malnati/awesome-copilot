---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
description: 'Boas praticas para desenvolver aplicacoes com Spring Boot e Kotlin.'
---

# Boas Praticas de Spring Boot com Kotlin

Seu objetivo e me ajudar a escrever aplicacoes Spring Boot de alta qualidade e idiomaticas usando Kotlin.

## Setup e Estrutura do Projeto

- **Build Tool:** Use Maven (`pom.xml`) ou Gradle (`build.gradle`) com os plugins Kotlin (`kotlin-maven-plugin` ou `org.jetbrains.kotlin.jvm`).
- **Kotlin Plugins:** Para JPA, habilite o plugin `kotlin-jpa` para tornar classes de entidade `open` automaticamente sem boilerplate.
- **Starters:** Use Spring Boot starters (por exemplo, `spring-boot-starter-web`, `spring-boot-starter-data-jpa`) como de costume.
- **Package Structure:** Organize o codigo por feature/dominio (por exemplo, `com.example.app.order`, `com.example.app.user`) em vez de por camada.

## Dependency Injection e Components

- **Primary Constructors:** Sempre use o construtor primario para injecao de dependencias obrigatorias. E a abordagem mais idiomatica e concisa em Kotlin.
- **Immutability:** Declare dependencias como `private val` no construtor primario. Prefira `val` em vez de `var` para promover imutabilidade.
- **Component Stereotypes:** Use `@Service`, `@Repository` e `@RestController` como faria em Java.

## Configuracao

- **Externalized Configuration:** Use `application.yml` pela legibilidade e estrutura hierarquica.
- **Type-Safe Properties:** Use `@ConfigurationProperties` com `data class` para criar objetos de configuracao imutaveis e type-safe.
- **Profiles:** Use Spring Profiles (`application-dev.yml`, `application-prod.yml`) para configuracoes por ambiente.
- **Secrets Management:** Nunca deixe secrets hardcoded. Use variaveis de ambiente ou uma ferramenta dedicada como HashiCorp Vault ou AWS Secrets Manager.

## Web Layer (Controllers)

- **RESTful APIs:** Desenhe endpoints RESTful claros e consistentes.
- **Data Classes para DTOs:** Use `data class` do Kotlin para todos os DTOs. Isso fornece `equals()`, `hashCode()`, `toString()` e `copy()` automaticamente e promove imutabilidade.
- **Validation:** Use Java Bean Validation (JSR 380) com anotacoes (`@Valid`, `@NotNull`, `@Size`) nas DTOs.
- **Error Handling:** Implemente um handler global com `@ControllerAdvice` e `@ExceptionHandler` para respostas consistentes.

## Service Layer

- **Business Logic:** Encapsule a logica de negocio em classes `@Service`.
- **Statelessness:** Services devem ser stateless.
- **Transaction Management:** Use `@Transactional` em metodos de service. Em Kotlin, pode ser aplicado no nivel de classe ou funcao.

## Data Layer (Repositories)

- **JPA Entities:** Defina entidades como classes. Lembre-se de que devem ser `open`. Recomenda-se usar o plugin `kotlin-jpa` para lidar com isso automaticamente.
- **Null Safety:** Use o null-safety do Kotlin (`?`) para deixar claro quais campos sao opcionais ou obrigatorios no nivel de tipo.
- **Spring Data JPA:** Use repositorios Spring Data JPA estendendo `JpaRepository` ou `CrudRepository`.
- **Coroutines:** Para apps reativas, use o suporte do Spring Boot a Kotlin Coroutines na camada de dados.

## Logging

- **Companion Object Logger:** A forma idiomatica de declarar um logger e em um companion object.
  ```kotlin
  companion object {
      private val logger = LoggerFactory.getLogger(MyClass::class.java)
  }
  ```
- **Parameterized Logging:** Use mensagens parametrizadas (`logger.info("Processing user {}...", userId)`) para performance e clareza.

## Testing

- **JUnit 5:** JUnit 5 e o default e funciona bem com Kotlin.
- **Idiomatic Testing Libraries:** Para testes mais fluent e idiomaticos, considere **Kotest** para assertivas e **MockK** para mocking. Sao projetados para Kotlin e oferecem sintaxe mais expressiva.
- **Test Slices:** Use anotacoes como `@WebMvcTest` ou `@DataJpaTest` para testar partes especificas da aplicacao.
- **Testcontainers:** Use Testcontainers para testes de integracao confiaveis com bancos reais, brokers de mensagem, etc.

## Coroutines e Programacao Assincrona

- **`suspend` functions:** Para codigo assincrono non-blocking, use `suspend` functions em controllers e services. Spring Boot tem excelente suporte a coroutines.
- **Structured Concurrency:** Use `coroutineScope` ou `supervisorScope` para gerenciar o ciclo de vida das coroutines.
