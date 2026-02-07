---
agent: 'agent'
description: 'Crie um esqueleto de projeto Spring Boot em Java'
---

# Criar prompt de projeto Spring Boot Java

- Garanta que voce tenha o seguinte software instalado no sistema:

  - Java 21
  - Docker
  - Docker Compose

- Se precisar customizar o nome do projeto, altere `artifactId` e `packageName` em [download-spring-boot-project-template](./create-spring-boot-java-project.prompt.md#download-spring-boot-project-template)

- Se precisar atualizar a versao do Spring Boot, altere `bootVersion` em [download-spring-boot-project-template](./create-spring-boot-java-project.prompt.md#download-spring-boot-project-template)

## Verificar versao do Java

- Execute o comando abaixo no terminal e confira a versao do Java

```shell
java -version
```

## Baixar template de projeto Spring Boot

- Execute o comando abaixo no terminal para baixar o template

```shell
curl https://start.spring.io/starter.zip \
  -d artifactId=${input:projectName:demo-java} \
  -d bootVersion=3.4.5 \
  -d dependencies=lombok,configuration-processor,web,data-jpa,postgresql,data-redis,data-mongodb,validation,cache,testcontainers \
  -d javaVersion=21 \
  -d packageName=com.example \
  -d packaging=jar \
  -d type=maven-project \
  -o starter.zip
```

## Descompactar o arquivo baixado

- Execute o comando abaixo para descompactar o template

```shell
unzip starter.zip -d ./${input:projectName:demo-java}
```

## Remover o zip baixado

- Execute o comando abaixo para remover o arquivo

```shell
rm -f starter.zip
```

## Entrar no diretorio do projeto

- Execute o comando abaixo para entrar no diretorio do projeto

```shell
cd ${input:projectName:demo-java}
```

## Adicionar dependencias extras

- Insira as dependencias `springdoc-openapi-starter-webmvc-ui` e `archunit-junit5` no `pom.xml`

```xml
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.8.6</version>
</dependency>
<dependency>
  <groupId>com.tngtech.archunit</groupId>
  <artifactId>archunit-junit5</artifactId>
  <version>1.2.1</version>
  <scope>test</scope>
</dependency>
```

## Adicionar configuracoes do SpringDoc, Redis, JPA e MongoDB

- Insira as configuracoes do SpringDoc no `application.properties`

```properties
# SpringDoc configurations
springdoc.swagger-ui.doc-expansion=none
springdoc.swagger-ui.operations-sorter=alpha
springdoc.swagger-ui.tags-sorter=alpha
```

- Insira as configuracoes do Redis no `application.properties`

```properties
# Redis configurations
spring.data.redis.host=localhost
spring.data.redis.port=6379
spring.data.redis.password=rootroot
```

- Insira as configuracoes do JPA no `application.properties`

```properties
# JPA configurations
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=rootroot
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

- Insira as configuracoes do MongoDB no `application.properties`

```properties
# MongoDB configurations
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.authentication-database=admin
spring.data.mongodb.username=root
spring.data.mongodb.password=rootroot
spring.data.mongodb.database=test
```

## Adicionar `docker-compose.yaml` com Redis, PostgreSQL e MongoDB

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

## Adicionar arquivo `.gitignore`

- Insira os diretorios `redis_data`, `postgres_data` e `mongo_data` no `.gitignore`

## Rodar Maven test

- Rode o comando de teste do Maven para checar se o projeto esta funcionando

```shell
./mvnw clean test
```

## Rodar Maven run (Opcional)

- (Opcional) `docker-compose up -d` para iniciar os servicos, `./mvnw spring-boot:run` para rodar o projeto, `docker-compose rm -sf` para parar os servicos.

## Vamos fazer isso passo a passo
