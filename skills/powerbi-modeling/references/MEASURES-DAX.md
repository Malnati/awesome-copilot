# DAX Measures and Naming Conventions

## Convencoes de Nomeacao

### Regras Gerais
- Use nomes legiveis (espacos permitidos)
- Seja descritivo: `Total Sales Amount` e nao `TSA`
- Evite abreviacoes a menos que sejam universalmente entendidas
- Use capitalizacao consistente (Title Case recomendado)
- Evite caracteres especiais exceto espacos

### Nomeacao de Tabelas
| Tipo | Convencao | Exemplo |
|------|-----------|---------|
| Dimension | Substantivo singular | Customer, Product, Date |
| Fact | Processo de negocio | Sales, Orders, Inventory |
| Bridge | Nomes combinados | CustomerAccount, ProductCategory |
| Measure Table | Prefixo com underscore | _Measures, _KPIs |

### Nomeacao de Colunas
| Tipo | Convencao | Exemplo |
|------|-----------|---------|
| Keys | Sufixo com "Key" ou "ID" | CustomerKey, ProductID |
| Dates | Sufixo com "Date" | OrderDate, ShipDate |
| Amounts | Descritivo com dica de unidade | SalesAmount, QuantitySold |
| Flags | Prefixo com "Is" ou "Has" | IsActive, HasDiscount |

### Nomeacao de Measures
| Tipo | Convencao | Exemplo |
|------|-----------|---------|
| Aggregations | Verbo + Substantivo | Total Sales, Count of Orders |
| Ratios | X per Y ou X Rate | Sales per Customer, Conversion Rate |
| Time Intelligence | Periodo + Metrica | YTD Sales, PY Total Sales |
| Comparisons | Metrica + vs + Baseline | Sales vs Budget, Growth vs PY |

## Explicit vs Implicit Measures

### Sempre Crie Measures Explicitas Para:
1. Metricas de negocio chave que usuarios consultarao
2. Calculos complexos com manipulacao de filtros
3. Measures usadas em MDX (Excel PivotTables)
4. Agregacao controlada (evita soma de medias)

### Implicit Measures (Agregacoes de Coluna)
- Aceitaveis para exploracao simples
- Defina SummarizeBy corretamente:
  - Amounts: Sum
  - Keys/IDs: None (Do Not Summarize)
  - Rates/Prices: None ou Average

## Padroes de Measure

### Agregacoes Basicas
```dax
Total Sales = SUM(Sales[SalesAmount])
Order Count = COUNTROWS(Sales)
Average Order Value = DIVIDE([Total Sales], [Order Count])
Distinct Customers = DISTINCTCOUNT(Sales[CustomerKey])
```

### Time Intelligence (Requer Date Table)
```dax
YTD Sales = TOTALYTD([Total Sales], 'Date'[Date])
MTD Sales = TOTALMTD([Total Sales], 'Date'[Date])
PY Sales = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))
YoY Growth = DIVIDE([Total Sales] - [PY Sales], [PY Sales])
```

### Calculos Percentuais
```dax
Sales % of Total = 
DIVIDE(
    [Total Sales],
    CALCULATE([Total Sales], REMOVEFILTERS(Product))
)

Margin % = DIVIDE([Gross Profit], [Total Sales])
```

### Totais Acumulados
```dax
Running Total = 
CALCULATE(
    [Total Sales],
    FILTER(
        ALL('Date'),
        'Date'[Date] <= MAX('Date'[Date])
    )
)
```

## Referencias de Coluna

### Boa Pratica: Sempre Qualifique Nomes de Coluna
```dax
// GOOD - Fully qualified
Sales Amount = SUM(Sales[SalesAmount])

// BAD - Unqualified (can cause ambiguity)
Sales Amount = SUM([SalesAmount])
```

### Referencias de Measures: Nunca Qualifique
```dax
// GOOD - Unqualified measure
YTD Sales = TOTALYTD([Total Sales], 'Date'[Date])

// BAD - Qualified measure (breaks if home table changes)
YTD Sales = TOTALYTD(Sales[Total Sales], 'Date'[Date])
```

## Documentacao

### Descricoes de Measure
Sempre adicione descricoes explicando:
- O que a measure calcula
- Contexto/uso de negocio
- Qualquer premissa importante

```
measure_operations(
  operation: "Update",
  definitions: [{
    name: "Total Sales",
    tableName: "Sales",
    description: "Sum of all completed sales transactions. Excludes returns and cancelled orders."
  }]
)
```

### Strings de Formatacao
| Tipo de Dado | String de Formatacao | Exemplo de Saida |
|-----------|---------------|----------------|
| Currency | $#,##0.00 | $1,234.56 |
| Percentage | 0.0% | 12.3% |
| Whole Number | #,##0 | 1,234 |
| Decimal | #,##0.00 | 1,234.56 |

## Display Folders

Organize measures em grupos logicos:
```
measure_operations(
  operation: "Update",
  definitions: [{
    name: "YTD Sales",
    tableName: "_Measures",
    displayFolder: "Time Intelligence\\Year"
  }]
)
```

Estrutura comum de pastas:
```
_Measures
├── Sales
│   ├── Total Sales
│   └── Average Sale
├── Time Intelligence
│   ├── Year
│   │   ├── YTD Sales
│   │   └── PY Sales
│   └── Month
│       └── MTD Sales
└── Ratios
    ├── Margin %
    └── Conversion Rate
```

## Variaveis para Performance

Use variaveis para:
- Evitar recalcular a mesma expressao
- Melhorar legibilidade
- Habilitar debugging

```dax
Gross Margin % = 
VAR TotalSales = [Total Sales]
VAR TotalCost = [Total Cost]
VAR GrossProfit = TotalSales - TotalCost
RETURN
    DIVIDE(GrossProfit, TotalSales)
```

## Checklist de Validacao

- [ ] Todas as metricas de negocio chave tem measures explicitas
- [ ] Measures tem nomes claros e descritivos
- [ ] Measures tem descricoes
- [ ] Format strings apropriadas aplicadas
- [ ] Display folders organizam measures relacionadas
- [ ] Referencias de coluna sao totalmente qualificadas
- [ ] Referencias de measure nao sao qualificadas
- [ ] Variaveis usadas para calculos complexos