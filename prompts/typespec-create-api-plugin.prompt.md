---
mode: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Crie um plugin de API para Microsoft 365 Copilot usando TypeSpec com schema, operations e adaptive cards'
model: 'gpt-4.1'
tags: [typespec, m365-copilot, api-plugin]
---

# Criar API Plugin com TypeSpec

Crie um plugin de API para Microsoft 365 Copilot usando TypeSpec com schema, operations e adaptive cards.

## Definir o Service

Use `@service` e `@server` para definir o service:

```typescript
@service
@server("https://api.example.com")
namespace MyPlugin {
  // Operations here
}
```

## Adicionar Operations

### GET Example
```typescript
@route("/items")
@get op listItems(): Item[];
```

### POST Example
```typescript
@route("/items")
@post op createItem(@body item: CreateItemRequest): Item;
```

## Declarar Models

```typescript
model Item {
  id: integer;
  title: string;
  description?: string;
}

model CreateItemRequest {
  title: string;
  description?: string;
}
```

## Adicionar Adaptive Cards

```typescript
@route("/items")
@card(#{
  dataPath: "$",
  title: "$.title",
  file: "item-card.json"
})
@get op listItems(): Item[];
```

## Testing Prompts

- "List all items"
- "Create an item with title 'Test'"
- "Get item 42"

## Boas Praticas

- Use nomes claros em operations e modelos
- Documente parametros com comentarios JSDoc
- Use `@visibility(Lifecycle.Read)` para campos read-only
- Use union types para enums
- Mantenha schemas simples e consistentes
