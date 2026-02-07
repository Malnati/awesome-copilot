---
name: powerbi-modeling
description: 'Assistente de modelagem semantica Power BI para criar modelos de dados otimizados. Use ao trabalhar com modelos semanticos Power BI, criar measures, desenhar star schemas, configurar relacionamentos, implementar RLS ou otimizar performance do modelo. Dispara em perguntas sobre calculos DAX, relacionamentos entre tabelas, design de dimensao/fato, convencoes de nomeacao, documentacao do modelo, cardinalidade, direcao de cross-filter, calculation groups e boas praticas de modelagem. Sempre conecte ao modelo ativo primeiro usando tools MCP de power-bi-modeling para entender a estrutura antes de orientar.'
---

# Modelagem Semantica Power BI

Guie usuarios na criacao de modelos semanticos Power BI otimizados e bem documentados seguindo boas praticas da Microsoft.

## Quando Usar Esta Skill

Use esta skill quando usuarios perguntarem sobre:
- Criar ou otimizar modelos semanticos Power BI
- Desenhar star schemas (tabelas dimensao/fato)
- Escrever DAX measures ou colunas calculadas
- Configurar relacionamentos de tabela (cardinalidade, cross-filter)
- Implementar row-level security (RLS)
- Convencoes de nomeacao para tabelas, colunas, measures
- Adicionar descricoes e documentacao a modelos
- Tuning de performance e otimizacao
- Calculation groups e field parameters
- Validacao de modelos e checagens de boas praticas

**Frases gatilho:** "create a measure", "add relationship", "star schema", "optimize model", "DAX formula", "RLS", "naming convention", "model documentation", "cardinality", "cross-filter"

## Pre-requisitos

### Tools Necessarias
- **Power BI Modeling MCP Server**: Necessario para conectar e modificar modelos semanticos
  - Habilita: connection_operations, table_operations, measure_operations, relationship_operations, etc.
  - Deve estar configurado e rodando para interagir com modelos

### Dependencias Opcionais
- **Microsoft Learn MCP Server**: Recomendado para pesquisar boas praticas mais recentes
  - Habilita: microsoft_docs_search, microsoft_docs_fetch
  - Use para cenarios complexos, novas features e documentacao oficial

## Workflow

### 1. Conectar e Analisar Primeiro

Antes de qualquer orientacao de modelagem, sempre examine o estado atual do modelo:

```
1. List connections: connection_operations(operation: "ListConnections")
2. If no connection, check for local instances: connection_operations(operation: "ListLocalInstances")
3. Connect to the model (Desktop or Fabric)
4. Get model overview: model_operations(operation: "Get")
5. List tables: table_operations(operation: "List")
6. List relationships: relationship_operations(operation: "List")
7. List measures: measure_operations(operation: "List")
```

### 2. Avaliar Saude do Modelo

Apos conectar, avalie o modelo contra boas praticas:

- **Star Schema**: Tabelas estao classificadas como dimensao ou fato?
- **Relacionamentos**: Cardinalidade correta? Minimo de filtros bidirecionais?
- **Nomeacao**: Convencoes consistentes e legiveis?
- **Documentacao**: Tabelas, colunas e measures tem descricoes?
- **Measures**: Measures explicitas para calculos-chave?
- **Campos ocultos**: Colunas tecnicas estao ocultas do report view?

### 3. Fornecer Orientacao Direcionada

Com base na analise, guie melhorias usando referencias:
- Design de star schema: veja [STAR-SCHEMA.md](references/STAR-SCHEMA.md)
- Configuracao de relacionamentos: veja [RELATIONSHIPS.md](references/RELATIONSHIPS.md)
- Measures DAX e nomeacao: veja [MEASURES-DAX.md](references/MEASURES-DAX.md)
- Otimizacao de performance: veja [PERFORMANCE.md](references/PERFORMANCE.md)
- Row-level security: veja [RLS.md](references/RLS.md)

## Referencia Rapida: Checklist de Qualidade do Modelo

| Area | Boa Pratica |
|------|--------------|
| Tabelas | Classificacao clara entre dimensao e fato |
| Nomeacao | Legivel para humanos: `Customer Name` e nao `CUST_NM` |
| Descricoes | Tabelas, colunas e measures documentadas |
| Measures | Measures DAX explicitas para metricas de negocio |
| Relacionamentos | Um-para-muitos de dimensao para fato |
| Cross-filter | Direcao unica salvo quando necessario |
| Campos ocultos | Oculte chaves tecnicas, IDs do report view |
| Tabela de datas | Tabela de datas dedicada e marcada |

## Referencia de MCP Tools

Use estas operacoes do Power BI Modeling MCP:

| Categoria de Operacao | Operacoes-chave |
|-------------------|----------------|
| `connection_operations` | Connect, ListConnections, ListLocalInstances, ConnectFabric |
| `model_operations` | Get, GetStats, ExportTMDL |
| `table_operations` | List, Get, Create, Update, GetSchema |
| `column_operations` | List, Get, Create, Update (descriptions, hidden, format) |
| `measure_operations` | List, Get, Create, Update, Move |
| `relationship_operations` | List, Get, Create, Update, Activate, Deactivate |
| `dax_query_operations` | Execute, Validate |
| `calculation_group_operations` | List, Create, Update |
| `security_role_operations` | List, Create, Update, GetEffectivePermissions |

## Tarefas Comuns

### Adicionar Measure com Descricao
```
measure_operations(
  operation: "Create",
  definitions: [{
    name: "Total Sales",
    tableName: "Sales",
    expression: "SUM(Sales[Amount])",
    formatString: "$#,##0",
    description: "Sum of all sales amounts"
  }]
)
```

### Atualizar Descricao de Coluna
```
column_operations(
  operation: "Update",
  definitions: [{
    tableName: "Customer",
    name: "CustomerKey",
    description: "Unique identifier for customer dimension",
    isHidden: true
  }]
)
```

### Criar Relacionamento
```
relationship_operations(
  operation: "Create",
  definitions: [{
    fromTable: "Sales",
    fromColumn: "CustomerKey",
    toTable: "Customer",
    toColumn: "CustomerKey",
    crossFilteringBehavior: "OneDirection"
  }]
)
```

## Quando Usar Microsoft Learn MCP

Pesquise boas praticas atuais usando `microsoft_docs_search` para:
- Documentacao de funcoes DAX mais recentes
- Novas features e capacidades do Power BI
- Cenarios complexos de modelagem (SCD Type 2, many-to-many)
- Tecnicas de otimizacao de performance
- Padroes de implementacao de seguranca
