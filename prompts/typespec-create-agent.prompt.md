---
mode: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Crie um agente para Microsoft 365 Copilot usando TypeSpec com definicoes de capabilities, tools e prompts'
model: 'gpt-4.1'
tags: [typespec, m365-copilot, api-plugin, agent]
---

# Criar Agente com TypeSpec

Crie um agente para Microsoft 365 Copilot usando TypeSpec com definicoes de capabilities, tools e prompts.

## Definir o Agente

Use o decorator `@actions` no namespace para definir o agente e suas capacidades:

```typescript
@service
@server("https://api.example.com")
@actions(#{
  nameForHuman: "My Agent",
  descriptionForHuman: "Agent description",
  descriptionForModel: "Detailed agent description for AI model",
  iconUrl: "https://example.com/icon.png",
  contactEmail: "support@example.com"
})
namespace MyAgent {
  // Operations here
}
```

## Adicionar Operacoes

### GET Operation Example
```typescript
@route("/items")
@get op listItems(): Item[];
```

### POST Operation Example
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

## Adicionar Capabilities

### Confirmation
```typescript
@route("/items")
@post
@capabilities(#{
  confirmation: #{
    type: "AdaptiveCard",
    title: "Create Item",
    body: "Are you sure you want to create this item?"
  }
})
op createItem(@body item: CreateItemRequest): Item;
```

### Adaptive Card
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

- Use nomes claros em `@actions` e modelos
- Documente parametros com comentarios JSDoc
- Use `@visibility(Lifecycle.Read)` para campos read-only
- Use union types para enums
- Adicione confirmacao para operacoes destrutivas
