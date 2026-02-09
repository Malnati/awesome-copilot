---
mode: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Adicione operacoes GET, POST, PATCH e DELETE a um plugin de API TypeSpec com routing, parametros e adaptive cards adequados'
model: 'gpt-4.1'
tags: [typespec, m365-copilot, api-plugin, rest-operations, crud]
---

# Adicionar Operacoes de API TypeSpec

Adicione operacoes RESTful a um plugin de API TypeSpec existente para Microsoft 365 Copilot.

## Adicionando Operacoes GET

### GET Simples - Listar Todos os Itens
```typescript
/**
 * List all items.
 */
@route("/items")
@get op listItems(): Item[];
```

### GET com Query Parameter - Filtrar Resultados
```typescript
/**
 * List items filtered by criteria.
 * @param userId Optional user ID to filter items
 */
@route("/items")
@get op listItems(@query userId?: integer): Item[];
```

### GET com Path Parameter - Obter Um Item
```typescript
/**
 * Get a specific item by ID.
 * @param id The ID of the item to retrieve
 */
@route("/items/{id}")
@get op getItem(@path id: integer): Item;
```

### GET com Adaptive Card
```typescript
/**
 * List items with adaptive card visualization.
 */
@route("/items")
@card(#{
  dataPath: "$",
  title: "$.title",
  file: "item-card.json"
})
@get op listItems(): Item[];
```

**Crie o Adaptive Card** (`appPackage/item-card.json`):
```json
{
  "type": "AdaptiveCard",
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "version": "1.5",
  "body": [
    {
      "type": "Container",
      "$data": "${$root}",
      "items": [
        {
          "type": "TextBlock",
          "text": "**${if(title, title, 'N/A')}**",
          "wrap": true
        },
        {
          "type": "TextBlock",
          "text": "${if(description, description, 'N/A')}",
          "wrap": true
        }
      ]
    }
  ],
  "actions": [
    {
      "type": "Action.OpenUrl",
      "title": "View Details",
      "url": "https://example.com/items/${id}"
    }
  ]
}
```

## Adicionando Operacoes POST

### POST Simples - Criar Item
```typescript
/**
 * Create a new item.
 * @param item The item to create
 */
@route("/items")
@post op createItem(@body item: CreateItemRequest): Item;

model CreateItemRequest {
  title: string;
  description?: string;
  userId: integer;
}
```

### POST com Confirmation
```typescript
/**
 * Create a new item with confirmation.
 */
@route("/items")
@post
@capabilities(#{
  confirmation: #{
    type: "AdaptiveCard",
    title: "Create Item",
    body: """
    Are you sure you want to create this item?
      * **Title**: {{ function.parameters.item.title }}
      * **User ID**: {{ function.parameters.item.userId }}
    """
  }
})
op createItem(@body item: CreateItemRequest): Item;
```

## Adicionando Operacoes PATCH

### PATCH Simples - Atualizar Item
```typescript
/**
 * Update an existing item.
 * @param id The ID of the item to update
 * @param item The updated item data
 */
@route("/items/{id}")
@patch op updateItem(
  @path id: integer,
  @body item: UpdateItemRequest
): Item;

model UpdateItemRequest {
  title?: string;
  description?: string;
  status?: "active" | "completed" | "archived";
}
```

### PATCH com Confirmation
```typescript
/**
 * Update an item with confirmation.
 */
@route("/items/{id}")
@patch
@capabilities(#{
  confirmation: #{
    type: "AdaptiveCard",
    title: "Update Item",
    body: """
    Updating item #{{ function.parameters.id }}:
      * **Title**: {{ function.parameters.item.title }}
      * **Status**: {{ function.parameters.item.status }}
    """
  }
})
op updateItem(
  @path id: integer,
  @body item: UpdateItemRequest
): Item;
```

## Adicionando Operacoes DELETE

### DELETE Simples
```typescript
/**
 * Delete an item.
 * @param id The ID of the item to delete
 */
@route("/items/{id}")
@delete op deleteItem(@path id: integer): void;
```

### DELETE com Confirmation
```typescript
/**
 * Delete an item with confirmation.
 */
@route("/items/{id}")
@delete
@capabilities(#{
  confirmation: #{
    type: "AdaptiveCard",
    title: "Delete Item",
    body: """
    ⚠️ Are you sure you want to delete item #{{ function.parameters.id }}?
    This action cannot be undone.
    """
  }
})
op deleteItem(@path id: integer): void;
```

## Exemplo CRUD Completo

### Definir o Service e Models
```typescript
@service
@server("https://api.example.com")
@actions(#{
  nameForHuman: "Items API",
  descriptionForHuman: "Manage items",
  descriptionForModel: "Read, create, update, and delete items"
})
namespace ItemsAPI {
  
  // Models
  model Item {
    @visibility(Lifecycle.Read)
    id: integer;
    
    userId: integer;
    title: string;
    description?: string;
    status: "active" | "completed" | "archived";
    
    @format("date-time")
    createdAt: utcDateTime;
    
    @format("date-time")
    updatedAt?: utcDateTime;
  }

  model CreateItemRequest {
    userId: integer;
    title: string;
    description?: string;
  }

  model UpdateItemRequest {
    title?: string;
    description?: string;
    status?: "active" | "completed" | "archived";
  }

  // Operations
  @route("/items")
  @card(#{ dataPath: "$", title: "$.title", file: "item-card.json" })
  @get op listItems(@query userId?: integer): Item[];

  @route("/items/{id}")
  @card(#{ dataPath: "$", title: "$.title", file: "item-card.json" })
  @get op getItem(@path id: integer): Item;

  @route("/items")
  @post
  @capabilities(#{
    confirmation: #{
      type: "AdaptiveCard",
      title: "Create Item",
      body: "Creating: **{{ function.parameters.item.title }}**"
    }
  })
  op createItem(@body item: CreateItemRequest): Item;

  @route("/items/{id}")
  @patch
  @capabilities(#{
    confirmation: #{
      type: "AdaptiveCard",
      title: "Update Item",
      body: "Updating item #{{ function.parameters.id }}"
    }
  })
  op updateItem(@path id: integer, @body item: UpdateItemRequest): Item;

  @route("/items/{id}")
  @delete
  @capabilities(#{
    confirmation: #{
      type: "AdaptiveCard",
      title: "Delete Item",
      body: "⚠️ Delete item #{{ function.parameters.id }}?"
    }
  })
  op deleteItem(@path id: integer): void;
}
```

## Recursos Avancados

### Multiplos Query Parameters
```typescript
@route("/items")
@get op listItems(
  @query userId?: integer,
  @query status?: "active" | "completed" | "archived",
  @query limit?: integer,
  @query offset?: integer
): ItemList;

model ItemList {
  items: Item[];
  total: integer;
  hasMore: boolean;
}
```

### Header Parameters
```typescript
@route("/items")
@get op listItems(
  @header("X-API-Version") apiVersion?: string,
  @query userId?: integer
): Item[];
```

### Custom Response Models
```typescript
@route("/items/{id}")
@delete op deleteItem(@path id: integer): DeleteResponse;

model DeleteResponse {
  success: boolean;
  message: string;
  deletedId: integer;
}
```

### Error Responses
```typescript
model ErrorResponse {
  error: {
    code: string;
    message: string;
    details?: string[];
  };
}

@route("/items/{id}")
@get op getItem(@path id: integer): Item | ErrorResponse;
```

## Testing Prompts

Apos adicionar operacoes, teste com estes prompts:

**GET Operations:**
- "List all items and show them in a table"
- "Show me items for user ID 1"
- "Get the details of item 42"

**POST Operations:**
- "Create a new item with title 'My Task' for user 1"
- "Add an item: title 'New Feature', description 'Add login'"

**PATCH Operations:**
- "Update item 10 with title 'Updated Title'"
- "Change the status of item 5 to completed"

**DELETE Operations:**
- "Delete item 99"
- "Remove the item with ID 15"

## Boas Praticas

### Parameter Naming
- Use nomes descritivos: `userId` e nao `uid`
- Seja consistente entre operacoes
- Use parametros opcionais (`?`) para filtros

### Documentation
- Adicione comentarios JSDoc em todas as operacoes
- Descreva o que cada parametro faz
- Documente respostas esperadas

### Models
- Use `@visibility(Lifecycle.Read)` para campos read-only como `id`
- Use `@format("date-time")` para campos de data
- Use union types para enums: `"active" | "completed"`
- Torne campos opcionais explicitos com `?`

### Confirmations
- Sempre adicione confirmacoes em operacoes destrutivas (DELETE, PATCH)
- Mostre detalhes chave no body da confirmacao
- Use emoji de aviso (⚠️) para acoes irreversiveis

### Adaptive Cards
- Mantenha cards simples e focados
- Use conditional rendering com `${if(..., ..., 'N/A')}`
- Inclua botao de acao para proximos passos
- Teste data binding com respostas reais da API

### Routing
- Use convencoes RESTful:
  - `GET /items` - List
  - `GET /items/{id}` - Get one
  - `POST /items` - Create
  - `PATCH /items/{id}` - Update
  - `DELETE /items/{id}` - Delete
- Agrupe operacoes relacionadas no mesmo namespace
- Use nested routes para recursos hierarquicos

## Common Issues

### Issue: Parameter not showing in Copilot
**Solution**: Check parameter is properly decorated with `@query`, `@path`, or `@body`

### Issue: Adaptive card not rendering
**Solution**: Verify file path in `@card` decorator and check JSON syntax

### Issue: Confirmation not appearing
**Solution**: Ensure `@capabilities` decorator is properly formatted with confirmation object

### Issue: Model property not appearing in response
**Solution**: Check if property needs `@visibility(Lifecycle.Read)` or remove it if it should be writable
