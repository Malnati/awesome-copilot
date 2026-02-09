---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
description: 'Boas praticas para desenvolver aplicacoes com Spring Boot.'
---

# Boas Praticas de Spring Boot

Seu objetivo e me ajudar a escrever aplicacoes Spring Boot de alta qualidade seguindo boas praticas consolidadas.

## Setup e Estrutura do Projeto

- **Build Tool:** Use Maven (`pom.xml`) ou Gradle (`build.gradle`) para gerenciamento de dependencias.
- **Starters:** Use Spring Boot starters (por exemplo, `spring-boot-starter-web`, `spring-boot-starter-data-jpa`) para simplificar dependencias.
- **Package Structure:** Organize o codigo por feature/dominio (por exemplo, `com.example.app.order`, `com.example.app.user`) em vez de por camada (por exemplo, `com.example.app.controller`, `com.example.app.service`).

## Dependency Injection e Components

- **Constructor Injection:** Sempre use injecao via construtor para dependencias obrigatorias. Isso facilita testes e deixa dependencias explicitas.
- **Immutability:** Declare campos de dependencias como `private final`.
- **Component Stereotypes:** Use as anotacoes `@Component`, `@Service`, `@Repository` e `@Controller`/`@RestController` de forma apropriada para definir beans.

## Configuracao

- **Externalized Configuration:** Use `application.yml` (ou `application.properties`) para configuracao. YAML geralmente e preferido por legibilidade e estrutura hierarquica.
- **Type-Safe Properties:** Use `@ConfigurationProperties` para mapear configuracao em objetos Java fortemente tipados.
- **Profiles:** Use Spring Profiles (`application-dev.yml`, `application-prod.yml`) para gerenciar configuracoes por ambiente.
- **Secrets Management:** Nao deixe secrets hardcoded. Use variaveis de ambiente ou uma ferramenta dedicada como HashiCorp Vault ou AWS Secrets Manager.

## Web Layer (Controllers)

- **RESTful APIs:** Desenhe endpoints RESTful claros e consistentes.
- **DTOs (Data Transfer Objects):** Use DTOs para expor e consumir dados na camada de API. Nao exponha entidades JPA diretamente ao cliente.
- **Validation:** Use Java Bean Validation (JSR 380) com anotacoes (`@Valid`, `@NotNull`, `@Size`) nos DTOs para validar payloads de request.
- **Error Handling:** Implemente um handler global de excecoes com `@ControllerAdvice` e `@ExceptionHandler` para respostas consistentes.

## Service Layer

- **Business Logic:** Encapsule toda logica de negocio em classes `@Service`.
- **Statelessness:** Services devem ser stateless.
- **Transaction Management:** Use `@Transactional` em metodos de service para gerenciar transacoes de forma declarativa. Aplique no nivel mais granular necessario.

## Data Layer (Repositories)

- **Spring Data JPA:** Use repositorios Spring Data JPA estendendo `JpaRepository` ou `CrudRepository` para operacoes padrao.
- **Custom Queries:** Para queries complexas, use `@Query` ou a JPA Criteria API.
- **Projections:** Use DTO projections para buscar apenas os dados necessarios do banco.

## Logging

- **SLF4J:** Use a API SLF4J para logging.
- **Logger Declaration:** `private static final Logger logger = LoggerFactory.getLogger(MyClass.class);`
- **Parameterized Logging:** Use mensagens parametrizadas (`logger.info("Processing user {}...", userId);`) em vez de concatenacao para melhor performance.

## Testing

- **Unit Tests:** Escreva testes unitarios para services e components usando JUnit 5 e um framework de mocking como Mockito.
- **Integration Tests:** Use `@SpringBootTest` para testes de integracao que carregam o contexto Spring.
- **Test Slices:** Use anotacoes de test slice como `@WebMvcTest` (controllers) ou `@DataJpaTest` (repositories) para testar partes especificas de forma isolada.
- **Testcontainers:** Considere Testcontainers para testes de integracao confiaveis com bancos reais, brokers de mensagem, etc.

## Security

- **Spring Security:** Use Spring Security para autenticacao e autorizacao.
- **Password Encoding:** Sempre codifique senhas com um algoritmo forte como BCrypt.
- **Input Sanitization:** Previna SQL injection usando Spring Data JPA ou queries parametrizadas. Previna Cross-Site Scripting (XSS) codificando corretamente a saida.
