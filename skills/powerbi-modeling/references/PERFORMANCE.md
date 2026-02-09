# Otimizacao de Performance para Modelos Power BI

## Tecnicas de Reducao de Dados

### 1. Remova Colunas Desnecessarias
- Importe apenas colunas necessarias para relatorios
- Remova colunas de auditoria (CreatedBy, ModifiedDate) a menos que sejam necessarias
- Remova colunas duplicadas/redundantes

```
column_operations(operation: "List", filter: { tableNames: ["Sales"] })
// Review and remove unneeded columns
```

### 2. Remova Linhas Desnecessarias
- Filtre dados historicos para o periodo relevante
- Exclua transacoes canceladas/void se nao forem necessarias
- Aplique filtros no Power Query (nao em DAX)

### 3. Reduza Cardinalidade
Alta cardinalidade (muitos valores unicos) impacta:
- Tamanho do modelo
- Tempo de refresh
- Performance de queries

**Solucoes:**
| Column Type | Reduction Technique |
|-------------|---------------------|
| DateTime | Split into Date and Time columns |
| Decimal precision | Round to needed precision |
| Text with patterns | Extract common prefix/suffix |
| High-precision IDs | Use surrogate integer keys |

### 4. Otimize Tipos de Dados
| From | To | Benefit |
|------|-----|---------|
| DateTime | Date (if time not needed) | 8 bytes to 4 bytes |
| Decimal | Fixed Decimal | Better compression |
| Text with numbers | Whole Number | Much better compression |
| Long text | Shorter text | Reduces storage |

### 5. Agrupar e Sumarizar
Pré-agregue dados quando detalhe nao for necessario:
- Daily instead of transactional
- Monthly instead of daily
- Considere tabelas de agregacao para DirectQuery

## Otimizacao de Colunas

### Prefira Colunas do Power Query a Colunas Calculadas
| Approach | When to Use |
|----------|-------------|
| Power Query (M) | Can be computed at source, static values |
| Calculated Column (DAX) | Needs model relationships, dynamic logic |

Colunas do Power Query:
- Carregam mais rapido
- Comprimem melhor
- Usam menos memoria

### Evite Colunas Calculadas em Chaves de Relacionamento
Colunas calculadas DAX em relacionamentos:
- Nao conseguem usar indices
- Geram SQL complexo no DirectQuery
- Pioram a performance significativamente

**Use COMBINEVALUES para relacionamentos multi-coluna:**
```dax
// If you must use calculated column for composite key
CompositeKey = COMBINEVALUES(",", [Country], [City])
```

### Defina Summarization Apropriada
Previna agregacao acidental de colunas nao aditivas:
```
column_operations(
  operation: "Update",
  definitions: [{
    tableName: "Product",
    name: "UnitPrice",
    summarizeBy: "None"
  }]
)
```

## Otimizacao de Relacionamentos

### 1. Minimize Relacionamentos Bidirecionais
Cada relacionamento bidirecional:
- Aumenta a complexidade da query
- Pode criar caminhos ambiguos
- Reduz a performance

### 2. Evite Many-to-Many Quando Possivel
Relacionamentos many-to-many:
- Geram queries mais complexas
- Exigem mais memoria
- Podem produzir resultados inesperados

### 3. Reduza Cardinalidade de Relacionamento
Mantenha colunas de relacionamento com baixa cardinalidade:
- Use chaves inteiras em vez de texto
- Considere relacionamentos em granularity maior

## Otimizacao de DAX

### 1. Use Variaveis
```dax
// GOOD - Calculate once, use twice
Sales Growth = 
VAR CurrentSales = [Total Sales]
VAR PriorSales = [PY Sales]
RETURN DIVIDE(CurrentSales - PriorSales, PriorSales)

// BAD - Recalculates [Total Sales] and [PY Sales]
Sales Growth = 
DIVIDE([Total Sales] - [PY Sales], [PY Sales])
```

### 2. Evite FILTER com Tabelas Inteiras
```dax
// BAD - Iterates entire table
Sales High Value = 
CALCULATE([Total Sales], FILTER(Sales, Sales[Amount] > 1000))

// GOOD - Uses column reference
Sales High Value = 
CALCULATE([Total Sales], Sales[Amount] > 1000)
```

### 3. Use KEEPFILTERS Apropriadamente
```dax
// Respects existing filters
Sales with Filter = 
CALCULATE([Total Sales], KEEPFILTERS(Product[Category] = "Bikes"))
```

### 4. Prefira DIVIDE ao Operador de Divisao
```dax
// GOOD - Handles divide by zero
Margin % = DIVIDE([Profit], [Sales])

// BAD - Errors on zero
Margin % = [Profit] / [Sales]
```

## Otimizacao de DirectQuery

### 1. Minimize Colunas e Tabelas
Modelos DirectQuery:
- Consultam a fonte para cada visual
- Performance depende da fonte
- Minimize dados retornados

### 2. Evite Transformacoes Complexas no Power Query
- Transformacoes viram subqueries
- Queries nativas sao mais rapidas
- Materialize na fonte quando possivel

### 3. Mantenha Measures Simples Inicialmente
DAX complexo gera SQL complexo:
- Comece com agregacoes basicas
- Adicione complexidade gradualmente
- Monitore performance de queries

### 4. Desative Auto Date/Time
Para modelos DirectQuery, desative auto date/time:
- Cria tabelas calculadas ocultas
- Aumenta a complexidade do modelo
- Use date table explicita em vez disso

## Agregacoes

### User-Defined Aggregations
Pré-agregue fact tables para:
- Modelos muito grandes (bilhoes de linhas)
- DirectQuery/Import hibrido
- Padroes comuns de query

```
table_operations(
  operation: "Create",
  definitions: [{
    name: "SalesAgg",
    mode: "Import",
    mExpression: "..."
  }]
)
```

## Testes de Performance

### Use Performance Analyzer
1. Enable in Power BI Desktop
2. Start recording
3. Interact with visuals
4. Review DAX query times

### Monitor with DAX Studio
External tool for:
- Query timing
- Server timings
- Query plans

## Checklist de Validacao

- [ ] Colunas desnecessarias removidas
- [ ] Tipos de dados apropriados usados
- [ ] Colunas de alta cardinalidade tratadas
- [ ] Relacionamentos bidirecionais minimizados
- [ ] DAX usa variaveis para expressoes repetidas
- [ ] Sem FILTER em tabelas inteiras
- [ ] DIVIDE usado em vez do operador de divisao
- [ ] Auto date/time desativado para DirectQuery
- [ ] Performance testada com dados representativos
