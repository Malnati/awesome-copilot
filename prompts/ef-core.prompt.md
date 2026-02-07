---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'runCommands']
description: 'Obtenha boas praticas para Entity Framework Core'
---

# Boas Praticas de Entity Framework Core

Seu objetivo e me ajudar a seguir boas praticas ao trabalhar com Entity Framework Core.

## Design do Data Context

- Mantenha classes DbContext focadas e coesas
- Use constructor injection para options de configuracao
- Override OnModelCreating para configuracao via fluent API
- Separe configuracoes de entidade usando IEntityTypeConfiguration
- Considere usar o pattern DbContextFactory para apps console ou testes

## Design de Entidades

- Use chaves primarias significativas (considere natural vs surrogate keys)
- Implemente relacionamentos corretos (one-to-one, one-to-many, many-to-many)
- Use data annotations ou fluent API para constraints e validacoes
- Implemente propriedades de navegacao adequadas
- Considere usar owned entity types para value objects

## Performance

- Use AsNoTracking() para queries somente leitura
- Implemente paginacao para grandes conjuntos com Skip() e Take()
- Use Include() para eager load de entidades relacionadas quando necessario
- Considere projection (Select) para buscar apenas campos requeridos
- Use compiled queries para queries executadas com frequencia
- Evite problemas de N+1 incluindo dados relacionados corretamente

## Migrations

- Crie migrations pequenas e focadas
- Nomeie migrations de forma descritiva
- Verifique scripts SQL de migration antes de aplicar em producao
- Considere usar migration bundles para deployment
- Adicione data seeding via migrations quando apropriado

## Querying

- Use IQueryable com criterio e entenda quando a query executa
- Prefira LINQ tipado em vez de SQL bruto
- Use operadores adequados (Where, OrderBy, GroupBy)
- Considere funcoes de banco para operacoes complexas
- Implemente specifications pattern para queries reutilizaveis

## Change Tracking e Saving

- Use estrategias de change tracking apropriadas
- Agrupe chamadas de SaveChanges()
- Implemente concurrency control para cenarios multi-usuario
- Considere transacoes para multiplas operacoes
- Use lifetimes adequados de DbContext (scoped para web apps)

## Seguranca

- Evite SQL injection usando queries parametrizadas
- Implemente permissoes adequadas de acesso a dados
- Tenha cuidado com SQL bruto
- Considere criptografia para dados sensiveis
- Use migrations para gerenciar permissoes de usuario no banco

## Testing

- Use provider in-memory para unit tests
- Crie contexts de teste separados com SQLite para integration tests
- Mock DbContext e DbSet para unit tests puros
- Teste migrations em ambientes isolados
- Considere snapshot testing para mudancas de model

Ao revisar meu codigo EF Core, identifique issues e sugira melhorias conforme essas boas praticas.
