---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Crie endpoints ASP.NET Minimal API com documentação OpenAPI adequada'
---

## ASP.NET Minimal API com OpenAPI

Seu objetivo é me ajudar a criar endpoints ASP.NET Minimal API bem estruturados, com tipos corretos e documentação OpenAPI/Swagger abrangente.

## Organização da API

- Agrupe endpoints relacionados usando a extensão `MapGroup()`
- Use filtros de endpoint para preocupações transversais
- Estruture APIs maiores com classes de endpoint separadas
- Considere usar uma estrutura de pastas por feature para APIs complexas

## Tipos de Requisição e Resposta

- Defina DTOs/models explícitos para requisição e resposta
- Crie classes de modelo claras com atributos de validação adequados
- Use tipos record para objetos imutáveis de requisição/resposta
- Use nomes de propriedades significativos que estejam alinhados com padrões de design de API
- Aplique `[Required]` e outros atributos de validação para impor restrições
- Use ProblemDetailsService e StatusCodePages para obter respostas de erro padrão

## Manipulação de Tipos

- Use parâmetros de rota fortemente tipados com binding explícito
- Use `Results<T1, T2>` para representar múltiplos tipos de resposta
- Return `TypedResults` instead of `Results` for strongly-typed responses
- Leverage C# 10+ features like nullable annotations and init-only properties

## OpenAPI Documentation

- Use the built-in OpenAPI document support added in .NET 9
- Define operation summary and description
- Add operationIds using the `WithName` extension method
- Add descriptions to properties and parameters with `[Description()]`
- Set proper content types for requests and responses
- Use document transformers to add elements like servers, tags, and security schemes
- Use schema transformers to apply customizations to OpenAPI schemas
