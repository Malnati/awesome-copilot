````prompt
---
mode: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Adicione templates de resposta Adaptive Card a API plugins baseados em MCP para apresentacao visual de dados no Microsoft 365 Copilot'
model: 'gpt-4.1'
tags: [mcp, adaptive-cards, m365-copilot, api-plugin, response-templates]
---

# Criar Adaptive Cards para MCP Plugins

Adicione templates de resposta Adaptive Card a API plugins baseados em MCP para melhorar como os dados sao apresentados visualmente no Microsoft 365 Copilot.

## Tipos de Adaptive Card

### Static Response Templates
Use quando a API sempre retorna itens do mesmo tipo e o formato nao muda com frequencia.

Defina em `response_semantics.static_template` no ai-plugin.json:

```json
{
  "functions": [
    {
      "name": "GetBudgets",
      "description": "Returns budget details including name and available funds",
      "capabilities": {
        "response_semantics": {
          "data_path": "$/",
          "properties": {
            "title": "$.name",
            "subtitle": "$.availableFunds"
          },
          "static_template": {
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
                    "text": "Name: ${if(name, name, 'N/A')}",
                    "wrap": true
                  },
                  {
                    "type": "TextBlock",
                    "text": "Available funds: ${if(availableFunds, formatNumber(availableFunds, 2), 'N/A')}",
                    "wrap": true
                  }
                ]
              }
            ]
          }
        }
      }
    }
  ]
}
```

### Dynamic Response Templates
Use quando a API retorna varios tipos e cada item precisa de um template diferente.

**Configuracao do ai-plugin.json:**
```json
{
  "name": "GetTransactions",
  "description": "Returns transaction details with dynamic templates",
  "capabilities": {
    "response_semantics": {
      "data_path": "$.transactions",
      "properties": {
        "template_selector": "$.displayTemplate"
      }
    }
  }
}
```

**Resposta da API com templates embutidos:**
```json
{
  "transactions": [
    {
      "budgetName": "Fourth Coffee lobby renovation",
      "amount": -2000,
      "description": "Property survey for permit application",
      "expenseCategory": "permits",
      "displayTemplate": "$.templates.debit"
    },
    {
      "budgetName": "Fourth Coffee lobby renovation",
      "amount": 5000,
      "description": "Additional funds to cover cost overruns",
      "expenseCategory": null,
      "displayTemplate": "$.templates.credit"
    }
  ],
  "templates": {
    "debit": {
      "type": "AdaptiveCard",
      "version": "1.5",
      "body": [
        {
          "type": "TextBlock",
          "size": "medium",
          "weight": "bolder",
          "color": "attention",
          "text": "Debit"
        },
        {
          "type": "FactSet",
          "facts": [
            {
              "title": "Budget",
              "value": "${budgetName}"
            },
            {
              "title": "Amount",
              "value": "${formatNumber(amount, 2)}"
            },
            {
              "title": "Category",
              "value": "${if(expenseCategory, expenseCategory, 'N/A')}"
            },
            {
              "title": "Description",
              "value": "${if(description, description, 'N/A')}"
            }
          ]
        }
      ],
      "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
    },
    "credit": {
      "type": "AdaptiveCard",
      "version": "1.5",
      "body": [
        {
          "type": "TextBlock",
          "size": "medium",
          "weight": "bolder",
          "color": "good",
          "text": "Credit"
        },
        {
          "type": "FactSet",
          "facts": [
            {
              "title": "Budget",
              "value": "${budgetName}"
            },
            {
              "title": "Amount",
              "value": "${formatNumber(amount, 2)}"
            },
            {
              "title": "Description",
              "value": "${if(description, description, 'N/A')}"
            }
          ]
        }
      ],
      "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
    }
  }
}
```

### Combined Static and Dynamic Templates
Use static template como padrao quando o item nao tiver template_selector ou quando o valor nao resolver.

```json
{
  "capabilities": {
    "response_semantics": {
      "data_path": "$.items",
      "properties": {
        "title": "$.name",
        "template_selector": "$.templateId"
      },
      "static_template": {
        "type": "AdaptiveCard",
        "version": "1.5",
        "body": [
          {
            "type": "TextBlock",
            "text": "Default: ${name}",
            "wrap": true
          }
        ]
      }
    }
  }
}
```

## Response Semantics Properties

### data_path
Query JSONPath indicando onde os dados residem na resposta da API:
```json
"data_path": "$"           // Root of response
"data_path": "$.results"   // In results property
"data_path": "$.data.items"// Nested path
```

### properties
Mapeie campos da resposta para citacoes do Copilot:
```json
"properties": {
  "title": "$.name",            // Citation title
  "subtitle": "$.description",  // Citation subtitle
  "url": "$.link"               // Citation link
}
```

### template_selector
Propriedade em cada item indicando qual template usar:
```json
"template_selector": "$.displayTemplate"
```

## Adaptive Card Template Language

### Conditional Rendering
```json
{
  "type": "TextBlock",
  "text": "${if(field, field, 'N/A')}"  // Show field or 'N/A'
}
```

### Number Formatting
```json
{
  "type": "TextBlock",
  "text": "${formatNumber(amount, 2)}"  // Two decimal places
}
```

### Data Binding
```json
{
  "type": "Container",
  "$data": "${$root}",  // Break to root context
  "items": [ ... ]
}
```

### Conditional Display
```json
{
  "type": "Image",
  "url": "${imageUrl}",
  "$when": "${imageUrl != null}"  // Only show if imageUrl exists
}
```

## Card Elements

### TextBlock
```json
{
  "type": "TextBlock",
  "text": "Text content",
  "size": "medium",      // small, default, medium, large, extraLarge
  "weight": "bolder",    // lighter, default, bolder
  "color": "attention",  // default, dark, light, accent, good, warning, attention
  "wrap": true
}
```

### FactSet
```json
{
  "type": "FactSet",
  "facts": [
    {
      "title": "Label",
      "value": "Value"
    }
  ]
}
```

### Image
```json
{
  "type": "Image",
  "url": "https://example.com/image.png",
  "size": "medium",  // auto, stretch, small, medium, large
  "style": "default" // default, person
}
```

### Container
```json
{
  "type": "Container",
  "$data": "${items}",  // Iterate over array
  "items": [
    {
      "type": "TextBlock",
      "text": "${name}"
    }
  ]
}
```

### ColumnSet
```json
{
  "type": "ColumnSet",
  "columns": [
    {
      "type": "Column",
      "width": "auto",
      "items": [ ... ]
    },
    {
      "type": "Column",
      "width": "stretch",
      "items": [ ... ]
    }
  ]
}
```

### Actions
```json
{
  "type": "Action.OpenUrl",
  "title": "View Details",
  "url": "https://example.com/item/${id}"
}
```

## Boas Praticas de Design Responsivo

### Layouts de Coluna Unica
- Use coluna unica para viewports estreitos
- Evite layouts multi-coluna quando possivel
- Garanta que os cards funcionem no menor viewport

### Larguras Flexiveis
- Nao atribua larguras fixas aos elementos
- Use "auto" ou "stretch" para propriedades de width
- Permita que elementos redimensionem com o viewport
- Larguras fixas OK apenas para icones/avatares

### Texto e Imagens
- Evite colocar texto e imagens na mesma linha
- Excecao: pequenos icones ou avatares
- Use "wrap": true para conteudo de texto
- Teste em varios tamanhos de viewport

### Teste em Diferentes Hubs
Valide cards em:
- Teams (desktop e mobile)
- Word
- PowerPoint
- Varios tamanhos de viewport (contrair/expandir UI)

## Exemplo Completo

**ai-plugin.json:**
```json
{
  "functions": [
    {
      "name": "SearchProjects",
      "description": "Search for projects with status and details",
      "capabilities": {
        "response_semantics": {
          "data_path": "$.projects",
          "properties": {
            "title": "$.name",
            "subtitle": "$.status",
            "url": "$.projectUrl"
          },
          "static_template": {
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
                    "size": "medium",
                    "weight": "bolder",
                    "text": "${if(name, name, 'Untitled Project')}",
                    "wrap": true
                  },
                  {
                    "type": "FactSet",
                    "facts": [
                      {
                        "title": "Status",
                        "value": "${status}"
                      },
                      {
                        "title": "Owner",
                        "value": "${if(owner, owner, 'Unassigned')}"
                      },
                      {
                        "title": "Due Date",
                        "value": "${if(dueDate, dueDate, 'Not set')}"
                      },
                      {
                        "title": "Budget",
                        "value": "${if(budget, formatNumber(budget, 2), 'N/A')}"
                      }
                    ]
                  },
                  {
                    "type": "TextBlock",
                    "text": "${if(description, description, 'No description')}",
                    "wrap": true,
                    "separator": true
                  }
                ]
              }
            ],
            "actions": [
              {
                "type": "Action.OpenUrl",
                "title": "View Project",
                "url": "${projectUrl}"
              }
            ]
          }
        }
      }
    }
  ]
}
```

## Workflow

Pergunte ao usuario:
1. Que tipo de dados a API retorna?
2. Todos os itens sao do mesmo tipo (static) ou tipos diferentes (dynamic)?
3. Quais campos devem aparecer no card?
4. Deve haver actions (por exemplo, "View Details")?
5. Ha multiplos estados ou categorias exigindo templates diferentes?

Em seguida, gere:
- Configuracao response_semantics adequada
- Template static, dynamic, ou ambos
- Data binding correto com conditional rendering
- Layout responsivo de coluna unica
- Cenários de teste para validacao

## Resources

- [Adaptive Card Designer](https://adaptivecards.microsoft.com/designer) - Ferramenta visual de design
- [Adaptive Card Schema](https://adaptivecards.io/schemas/adaptive-card.json) - Referencia completa do schema
- [Template Language](https://learn.microsoft.com/en-us/adaptive-cards/templating/language) - Guia de sintaxe de binding
- [JSONPath](https://www.rfc-editor.org/rfc/rfc9535) - Sintaxe de consulta de path

## Common Patterns

### List with Images
```json
{
  "type": "Container",
  "$data": "${items}",
  "items": [
    {
      "type": "ColumnSet",
      "columns": [
        {
          "type": "Column",
          "width": "auto",
          "items": [
            {
              "type": "Image",
              "url": "${thumbnailUrl}",
              "size": "small",
              "$when": "${thumbnailUrl != null}"
            }
          ]
        },
        {
          "type": "Column",
          "width": "stretch",
          "items": [
            {
              "type": "TextBlock",
              "text": "${title}",
              "weight": "bolder",
              "wrap": true
            }
          ]
        }
      ]
    }
  ]
}
```

### Status Indicators
```json
{
  "type": "TextBlock",
  "text": "${status}",
  "color": "${if(status == 'Completed', 'good', if(status == 'In Progress', 'attention', 'default'))}"
}
```

### Currency Formatting
```json
{
  "type": "TextBlock",
  "text": "$${formatNumber(amount, 2)}"
}
```

````
