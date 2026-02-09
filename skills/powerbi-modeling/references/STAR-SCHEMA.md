# Star Schema Design for Power BI

## Visao Geral

Star schema e o padrao de design ideal para modelos semanticos do Power BI. Ele organiza os dados em:
- **Dimension tables**: Permitem filtros e agrupamentos (lado "one")
- **Fact tables**: Permitem sumarizacao (lado "many")

## Classificacao de Tabelas

### Dimension Tables
- Contem atributos descritivos para filtro/slice
- Possuem colunas de chave unicas (uma linha por entidade)
- Exemplos: Customer, Product, Date, Geography, Employee
- Convencao de nomes: Substantivo singular (`Customer`, `Product`)

### Fact Tables  
- Contem dados mensuraveis e quantitativos
- Possuem foreign keys para dimensoes
- Armazenam dados em granularity consistente (uma linha por transacao/evento)
- Exemplos: Sales, Orders, Inventory, WebVisits
- Convencao de nomes: Substantivo do processo de negocio (`Sales`, `Orders`)

## Principios de Design

### 1. Separar Dimensoes de Fatos
```
BAD:  Single denormalized "Sales" table with customer details
GOOD: "Sales" fact table + "Customer" dimension table
```

### 2. Grain Consistente
Cada linha em uma fact table representa a mesma coisa:
- Order line level (most common)
- Daily aggregation
- Monthly summary

Nunca misture grains em uma unica tabela.

### 3. Surrogate Keys
Adicione surrogate keys quando a origem nao tiver identificadores unicos:
```m
// Power Query: Add index column
= Table.AddIndexColumn(Source, "CustomerKey", 1, 1)
```

### 4. Date Dimension
Sempre crie uma tabela de data dedicada:
- Marque como date table no Power BI
- Inclua periodos fiscais se necessario
- Adicione colunas de datas relativas (IsCurrentMonth, IsPreviousYear)

```dax
Date = 
ADDCOLUMNS(
    CALENDAR(DATE(2020,1,1), DATE(2030,12,31)),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "MonthNum", MONTH([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "WeekDay", FORMAT([Date], "dddd")
)
```

## Tipos Especiais de Dimensao

### Dimensoes de Role-Playing
A mesma dimensao usada varias vezes (ex.: Date para OrderDate, ShipDate):
- Opcao 1: Duplicar a tabela (OrderDate, ShipDate tables)
- Opcao 2: Usar relacionamentos inativos com USERELATIONSHIP em DAX

### Slowly Changing Dimensions (Type 2)
Rastreie mudancas historicas com colunas de versao:
- StartDate, EndDate columns
- IsCurrent flag
- Requer pre-processamento no data warehouse

### Junk Dimensions
Combine flags de baixa cardinalidade em uma tabela:
```
OrderFlags dimension: IsRush, IsGift, IsOnline
```

### Degenerate Dimensions
Mantenha identificadores de transacao (OrderNumber, InvoiceID) na fact table.

## Anti-Patterns a Evitar

| Anti-Pattern | Problem | Solution |
|--------------|---------|----------|
| Wide denormalized tables | Poor performance, hard to maintain | Split into star schema |
| Snowflake (normalized dims) | Extra joins hurt performance | Flatten dimensions |
| Many-to-many without bridge | Ambiguous results | Add bridge/junction table |
| Mixed grain facts | Incorrect aggregations | Separate tables per grain |

## Checklist de Validacao

- [ ] Cada tabela e claramente dimension ou fact
- [ ] Fact tables tem foreign keys para todas as dimensoes relacionadas
- [ ] Dimensions tem colunas de chave unicas
- [ ] Date table existe e esta marcada
- [ ] Sem caminhos de relacionamento circulares
- [ ] Convencoes de nomes consistentes
