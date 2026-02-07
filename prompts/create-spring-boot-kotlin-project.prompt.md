---
agent: 'agent'
description: 'Crie um esqueleto de projeto Spring Boot em Kotlin'
---

# Criar prompt de projeto Spring Boot Kotlin

- Garanta que voce tenha o seguinte software instalado no sistema:

  - Java 21
  - Docker
  - Docker Compose

- Se precisar customizar o nome do projeto, altere `artifactId` e `packageName` em [download-spring-boot-project-template](./create-spring-boot-kotlin-project.prompt.md#download-spring-boot-project-template)

- Se precisar atualizar a versao do Spring Boot, altere `bootVersion` em [download-spring-boot-project-template](./create-spring-boot-kotlin-project.prompt.md#download-spring-boot-project-template)

## Verificar versao do Java

- Execute o comando abaixo no terminal e confira a versao do Java

```shell
java -version
```

## Baixar template de projeto Spring Boot

- Execute o comando abaixo no terminal para baixar o template

```shell
curl https://start.spring.io/starter.zip \
  -d artifactId=${input:projectName:demo-kotlin} \
  -d bootVersion=3.4.5 \
  -d dependencies=configuration-processor,webflux,data-r2dbc,postgresql,data-redis-reactive,data-mongodb-reactive,validation,cache,testcontainers \
  -d javaVersion=21 \
  -d language=kotlin \
  -d packageName=com.example \
  -d packaging=jar \
  -d type=gradle-project-kotlin \
  -o starter.zip
```

## Descompactar o arquivo baixado

- Execute o comando abaixo para descompactar o template

```shell
unzip starter.zip -d ./${input:projectName:demo-kotlin}
```

## Remover o zip baixado

- Execute o comando abaixo para remover o arquivo

```shell
rm -f starter.zip
```

## Descompactar o arquivo baixado

- Execute o comando abaixo para descompactar o template

```shell
unzip starter.zip -d ./${input:projectName:demo-kotlin}
```

## Adicionar dependencias extras

- Insira `springdoc-openapi-starter-webmvc-ui` e `archunit-junit5` no `build.gradle.kts`

```gradle.kts
dependencies {
  implementation("org.springdoc:springdoc-openapi-starter-webflux-ui:2.8.6")
  testImplementation("com.tngtech.archunit:archunit-junit5:1.2.1")
}
```

- Insira configuracoes do SpringDoc no `application.properties`

```properties
# SpringDoc configurations
springdoc.swagger-ui.doc-expansion=none
springdoc.swagger-ui.operations-sorter=alpha
springdoc.swagger-ui.tags-sorter=alpha
```

- Insira configuracoes do Redis no `application.properties`

```properties
# Redis configurations
spring.data.redis.host=localhost
spring.data.redis.port=6379
spring.data.redis.password=rootroot
```

- Insira configuracoes do R2DBC no `application.properties`

```properties
# R2DBC configurations
spring.r2dbc.url=r2dbc:postgresql://localhost:5432/postgres
spring.r2dbc.username=postgres
spring.r2dbc.password=rootroot

spring.sql.init.mode=always
spring.sql.init.platform=postgres
spring.sql.init.continue-on-error=true
```

- Insira configuracoes do MongoDB no `application.properties`

```properties
# MongoDB configurations
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.authentication-database=admin
spring.data.mongodb.username=root
spring.data.mongodb.password=rootroot
spring.data.mongodb.database=test
```

- Crie `docker-compose.yaml` na raiz do projeto e adicione os servicos: `redis:6`, `postgresql:17` e `mongo:8`.

  - O servico redis deve ter
    - password `rootroot`
    - mapping port 6379 to 6379
    - mounting volume `./redis_data` to `/data`
  - O servico postgresql deve ter
    - password `rootroot`
    - mapping port 5432 to 5432
    - mounting volume `./postgres_data` to `/var/lib/postgresql/data`
  - O servico mongo deve ter
    - initdb root username `root`
    - initdb root password `rootroot`
    - mapping port 27017 to 27017
    - mounting volume `./mongo_data` to `/data/db`

- Insira os diretorios `redis_data`, `postgres_data` e `mongo_data` no `.gitignore`

- Rode o comando de teste do Gradle para checar se o projeto esta funcionando

```shell
./gradlew clean test
```

- (Opcional) `docker-compose up -d` para iniciar os servicos, `./gradlew spring-boot:run` para rodar o projeto, `docker-compose rm -sf` para parar os servicos.

Vamos fazer isso passo a passo.
