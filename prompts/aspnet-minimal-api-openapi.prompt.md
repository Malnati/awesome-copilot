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
- Retorne `TypedResults` em vez de `Results` para respostas fortemente tipadas
- Aproveite recursos do C# 10+ como nullable annotations e propriedades init-only

## OpenAPI Documentation

- Use o suporte embutido a documentos OpenAPI adicionado no .NET 9
- Defina resumo e descrição da operação
- Adicione operationIds usando o método de extensão `WithName`
- Adicione descrições a propriedades e parâmetros com `[Description()]`
- Defina content types adequados para requisições e respostas
- Use document transformers para adicionar elementos como servers, tags e security schemes
- Use schema transformers para aplicar customizações a schemas OpenAPI
