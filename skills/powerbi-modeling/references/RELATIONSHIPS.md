# Relacionamentos no Power BI

## Propriedades de Relacionamento

### Cardinalidade
| Tipo | Caso de Uso | Notas |
|------|------------|-------|
| One-to-Many (*:1) | Dimensao para Fato | Mais comum, preferido |
| Many-to-One (1:*) | Fato para Dimensao | Igual acima, direcao invertida |
| One-to-One (1:1) | Extensions de dimensao | Use com parcimonia |
| Many-to-Many (*:*) | Bridge tables, cenarios complexos | Requer design cuidadoso |

### Direcao de Cross-Filter
| Configuracao | Comportamento | Quando Usar |
|---------|----------|-------------|
| Single | Filtros fluem de "one" para "many" | Padrao, melhor performance |
| Both | Filtros fluem em ambas as direcoes | Apenas quando necessario |

## Boas Praticas

### 1. Prefira Relacionamentos One-to-Many
```
Customer (1) --> (*) Sales
Product  (1) --> (*) Sales
Date     (1) --> (*) Sales
```

### 2. Use Cross-Filtering de Direcao Unica
Filtro bidirecional:
- Impacta negativamente a performance
- Pode criar caminhos de filtro ambiguos
- Pode produzir resultados inesperados

**Use bidirecional apenas quando:**
- Analise dimension-to-dimension via fact table
- Requisitos especificos de RLS

**Alternativa melhor:** Use CROSSFILTER em measures DAX:
```dax
Countries Sold = 
CALCULATE(
    DISTINCTCOUNT(Customer[Country]),
    CROSSFILTER(Customer[CustomerKey], Sales[CustomerKey], BOTH)
)
```

### 3. Um Caminho Ativo Entre Tabelas
- Apenas um relacionamento ativo entre duas tabelas
- Use USERELATIONSHIP para role-playing dimensions:

```dax
Sales by Ship Date = 
CALCULATE(
    [Total Sales],
    USERELATIONSHIP(Sales[ShipDate], Date[Date])
)
```

### 4. Evite Caminhos Ambiguos
Referencias circulares causam erros. Solucoes:
- Desativar um relacionamento
- Reestruturar o modelo
- Usar USERELATIONSHIP em measures

## Padroes de Relacionamento

### Star Schema Padrao
```
     [Date]
       |
[Product]--[Sales]--[Customer]
       |
   [Store]
```

### Dimensao de Role-Playing
```
[Date] --(active)-- [Sales.OrderDate]
   |
   +--(inactive)-- [Sales.ShipDate]
```

### Bridge Table (Many-to-Many)
```
[Customer]--(*)--[CustomerAccount]--(*)--[Account]
```

### Factless Fact Table
```
[Product]--[ProductPromotion]--[Promotion]
```
Usada para capturar relacionamentos sem measures.

## Criar Relacionamentos via MCP

### Listar Relacionamentos Atuais
```
relationship_operations(operation: "List")
```

### Criar Novo Relacionamento
```
relationship_operations(
  operation: "Create",
  definitions: [{
    fromTable: "Sales",
    fromColumn: "ProductKey",
    toTable: "Product", 
    toColumn: "ProductKey",
    crossFilteringBehavior: "OneDirection",
    isActive: true
  }]
)
```

### Desativar Relacionamento
```
relationship_operations(
  operation: "Deactivate",
  references: [{ name: "relationship-guid-here" }]
)
```

## Solucao de Problemas

### Erro "Ambiguous Path"
Existem multiplos caminhos ativos entre tabelas.
- Verifique: Multiplas fact tables compartilhando dimensions
- Solucao: Desative relacionamentos redundantes

### Bidirectional Not Allowed
Uma referencia circular seria criada.
- Solucao: Reestruture ou use DAX CROSSFILTER

### Relationship Not Detected
Colunas podem ter tipos de dados diferentes.
- Garanta que ambas colunas tenham tipos identicos
- Verifique espacos no final de chaves de texto

## Checklist de Validacao

- [ ] Todos os relacionamentos sao one-to-many quando possivel
- [ ] Cross-filter e de direcao unica por default
- [ ] Apenas um caminho ativo entre duas tabelas
- [ ] Role-playing dimensions usam relacionamentos inativos
- [ ] Sem caminhos de referencia circulares
- [ ] Colunas de chave tem tipos de dados correspondentes