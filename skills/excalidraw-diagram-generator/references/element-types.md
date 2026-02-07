# Guia de Tipos de Elemento do Excalidraw

Especificacoes detalhadas para cada tipo de elemento do Excalidraw, com exemplos visuais e casos de uso.

## Visao Geral dos Tipos de Elemento

| Tipo | Visual | Uso Principal | Suporte a Texto |
|------|--------|-------------|--------------|
| `rectangle` | □ | Boxes, containers, process steps | ✅ Yes |
| `ellipse` | ○ | Emphasis, terminals, states | ✅ Yes |
| `diamond` | ◇ | Decision points, choices | ✅ Yes |
| `arrow` | → | Directional flow, relationships | ❌ No (use separate text) |
| `line` | — | Connections, dividers | ❌ No |
| `text` | A | Labels, annotations, titles | ✅ (Its purpose) |

---

## Rectangle

**Melhor para:** Process steps, entities, data stores, components

### Propriedades

```typescript
{
  type: "rectangle",
  roundness: { type: 3 },  // Rounded corners
  text: "Step Name",       // Optional embedded text
  fontSize: 20,
  textAlign: "center",
  verticalAlign: "middle"
}
```

### Casos de Uso

| Cenario | Configuracao |
|----------|---------------|
| **Process step** | Green background (`#b2f2bb`), centered text |
| **Entity/Object** | Blue background (`#a5d8ff`), medium size |
| **System component** | Light color, descriptive text |
| **Data store** | Gray/white, database-like label |

### Diretrizes de Tamanho

| Conteudo | Largura | Altura |
|---------|-------|--------|
| Palavra unica | 120-150px | 60-80px |
| Frase curta (2-4 palavras) | 180-220px | 80-100px |
| Frase | 250-300px | 100-120px |

### Exemplo

```json
{
  "type": "rectangle",
  "x": 100,
  "y": 100,
  "width": 200,
  "height": 80,
  "backgroundColor": "#b2f2bb",
  "text": "Validate Input",
  "fontSize": 20,
  "textAlign": "center",
  "verticalAlign": "middle",
  "roundness": { "type": 3 }
}
```

---

## Ellipse

**Melhor para:** Start/end points, states, emphasis circles

### Propriedades

```typescript
{
  type: "ellipse",
  text: "Start",
  fontSize: 18,
  textAlign: "center",
  verticalAlign: "middle"
}
```

### Casos de Uso

| Cenario | Configuracao |
|----------|---------------|
| **Flow start** | Light green, "Start" text |
| **Flow end** | Light red, "End" text |
| **State** | Soft color, state name |
| **Highlight** | Bright color, emphasis text |

### Diretrizes de Tamanho

Para formas circulares, use `width === height`:

| Conteudo | Diametro |
|---------|----------|
| Icon/Symbol | 60-80px |
| Texto curto | 100-120px |
| Texto mais longo | 150-180px |

### Exemplo

```json
{
  "type": "ellipse",
  "x": 100,
  "y": 100,
  "width": 120,
  "height": 120,
  "backgroundColor": "#d0f0c0",
  "text": "Start",
  "fontSize": 18,
  "textAlign": "center",
  "verticalAlign": "middle"
}
```

---

## Diamond

**Melhor para:** Decision points, conditional branches

### Propriedades

```typescript
{
  type: "diamond",
  text: "Valid?",
  fontSize: 18,
  textAlign: "center",
  verticalAlign: "middle"
}
```

### Casos de Uso

| Cenario | Exemplo de Texto |
|----------|--------------|
| **Decisao Sim/Nao** | "Is Valid?", "Exists?" |
| **Escolha multipla** | "Type?", "Status?" |
| **Condicional** | "Score > 50?" |

### Diretrizes de Tamanho

Diamonds precisam de mais espaco que retangulos para o mesmo texto:

| Conteudo | Largura | Altura |
|---------|-------|--------|
| Yes/No | 120-140px | 120-140px |
| Short question | 160-180px | 160-180px |
| Longer question | 200-220px | 200-220px |

### Exemplo

```json
{
  "type": "diamond",
  "x": 100,
  "y": 100,
  "width": 150,
  "height": 150,
  "backgroundColor": "#ffe4a3",
  "text": "Valid?",
  "fontSize": 18,
  "textAlign": "center",
  "verticalAlign": "middle"
}
```

---

## Arrow

**Melhor para:** Flow direction, relationships, dependencies

### Propriedades

```typescript
{
  type: "arrow",
  points: [[0, 0], [endX, endY]],  // Relative coordinates
  roundness: { type: 2 },          // Curved
  startBinding: null,              // Or { elementId, focus, gap }
  endBinding: null
}
```

### Direcoes da Seta

#### Horizontal (Esquerda para Direita)

```json
{
  "x": 100,
  "y": 150,
  "width": 200,
  "height": 0,
  "points": [[0, 0], [200, 0]]
}
```

#### Vertical (Topo para Base)

```json
{
  "x": 200,
  "y": 100,
  "width": 0,
  "height": 150,
  "points": [[0, 0], [0, 150]]
}
```

#### Diagonal

```json
{
  "x": 100,
  "y": 100,
  "width": 200,
  "height": 150,
  "points": [[0, 0], [200, 150]]
}
```

### Arrow Styles

| Estilo | `strokeStyle` | `strokeWidth` | Caso de Uso |
|-------|---------------|---------------|----------|
| **Normal flow** | "solid" | 2 | Standard connections |
| **Optional/Weak** | "dashed" | 2 | Optional paths |
| **Important** | "solid" | 3-4 | Emphasized flow |
| **Dotted** | "dotted" | 2 | Indirect relationships |

### Adding Arrow Labels

Use separate text elements positioned near arrow midpoint:

```json
[
  {
    "type": "arrow",
    "id": "arrow1",
    "x": 100,
    "y": 150,
    "points": [[0, 0], [200, 0]]
  },
  {
    "type": "text",
    "x": 180,      // Near midpoint
    "y": 130,      // Above arrow
    "text": "sends",
    "fontSize": 14
  }
]
```

---

## Line

**Melhor para:** Non-directional connections, dividers, borders

### Propriedades

```typescript
{
  type: "line",
  points: [[0, 0], [x2, y2], [x3, y3], ...],
  roundness: null  // Or { type: 2 } for curved
}
```

### Casos de Uso

| Cenario | Configuracao |
|----------|---------------|
| **Divider** | Horizontal, thin stroke |
| **Border** | Closed path (polygon) |
| **Connection** | Multi-point path |
| **Underline** | Short horizontal line |

### Exemplo de Linha com Multiplos Pontos

```json
{
  "type": "line",
  "x": 100,
  "y": 100,
  "points": [
    [0, 0],
    [100, 50],
    [200, 0]
  ]
}
```

---

## Text

**Melhor para:** Labels, titles, annotations, standalone text

### Propriedades

```typescript
{
  type: "text",
  text: "Label text",
  fontSize: 20,
  fontFamily: 1,        // 1=Virgil, 2=Helvetica, 3=Cascadia
  textAlign: "left",
  verticalAlign: "top"
}
```

### Font Sizes by Purpose

| Purpose | Font Size |
|---------|-----------|
| **Main title** | 28-36 |
| **Section header** | 24-28 |
| **Element label** | 18-22 |
| **Annotation** | 14-16 |
| **Small note** | 12-14 |

### Width/Height Calculation

```javascript
// Approximate width
const width = text.length * fontSize * 0.6;

// Approximate height (single line)
const height = fontSize * 1.2;

// Multi-line
const lines = text.split('\n').length;
const height = fontSize * 1.2 * lines;
```

### Text Positioning

| Posicao | textAlign | verticalAlign | Caso de Uso |
|----------|-----------|---------------|----------|
| **Top-left** | "left" | "top" | Labels padrao |
| **Centered** | "center" | "middle" | Titles |
| **Bottom-right** | "right" | "bottom" | Footnotes |

### Exemplo: Title

```json
{
  "type": "text",
  "x": 100,
  "y": 50,
  "width": 400,
  "height": 40,
  "text": "System Architecture",
  "fontSize": 32,
  "fontFamily": 2,
  "textAlign": "center",
  "verticalAlign": "top"
}
```

### Exemplo: Annotation

```json
{
  "type": "text",
  "x": 150,
  "y": 200,
  "width": 100,
  "height": 20,
  "text": "User input",
  "fontSize": 14,
  "fontFamily": 1,
  "textAlign": "left",
  "verticalAlign": "top"
}
```

---

## Combining Elements

### Pattern: Labeled Box

```json
[
  {
    "type": "rectangle",
    "id": "box1",
    "x": 100,
    "y": 100,
    "width": 200,
    "height": 100,
    "text": "Component",
    "textAlign": "center",
    "verticalAlign": "middle"
  }
]
```

### Pattern: Connected Boxes

```json
[
  {
    "type": "rectangle",
    "id": "box1",
    "x": 100,
    "y": 100,
    "width": 150,
    "height": 80,
    "text": "Step 1"
  },
  {
    "type": "arrow",
    "id": "arrow1",
    "x": 250,
    "y": 140,
    "points": [[0, 0], [100, 0]]
  },
  {
    "type": "rectangle",
    "id": "box2",
    "x": 350,
    "y": 100,
    "width": 150,
    "height": 80,
    "text": "Step 2"
  }
]
```

### Pattern: Decision Tree

```json
[
  {
    "type": "diamond",
    "id": "decision",
    "x": 100,
    "y": 100,
    "width": 140,
    "height": 140,
    "text": "Valid?"
  },
  {
    "type": "arrow",
    "id": "yes-arrow",
    "x": 240,
    "y": 170,
    "points": [[0, 0], [60, 0]]
  },
  {
    "type": "text",
    "id": "yes-label",
    "x": 250,
    "y": 150,
    "text": "Yes",
    "fontSize": 14
  },
  {
    "type": "rectangle",
    "id": "yes-box",
    "x": 300,
    "y": 140,
    "width": 120,
    "height": 60,
    "text": "Process"
  }
]
```

---

## Resumo

| When you need... | Use this element |
|------------------|------------------|
| Process box | `rectangle` with text |
| Decision point | `diamond` with question |
| Flow direction | `arrow` |
| Start/End | `ellipse` |
| Title/Header | `text` (large font) |
| Annotation | `text` (small font) |
| Non-directional link | `line` |
| Divider | `line` (horizontal) |