# Seguranca em Nivel de Linha (RLS) no Power BI

## Visao Geral

Seguranca em Nivel de Linha (RLS) restringe o acesso aos dados no nivel de linha com base na identidade do usuario. Os usuarios veem apenas os dados que estao autorizados a visualizar.

## Principios de Design

### 1. Filtre em Tabelas de Dimensao
Aplique RLS em dimensions, nao em fact tables:
- Mais eficiente (tabelas menores)
- Filtros propagam pelos relacionamentos
- Mais facil de manter

```dax
// On Customer dimension - filters propagate to Sales
[Region] = "West"
```

### 2. Crie Roles Minimas
Evite muitas combinacoes de roles:
- Cada role = cache separado
- Roles sao aditivas (uniao, nao interseccao)
- Consolide quando possivel

### 3. Use Dynamic RLS Quando Possivel
Regras orientadas a dados escalam melhor:
- Mapeamento de usuarios em uma tabela
- USERPRINCIPALNAME() para identidade
- Sem mudancas de role quando usuarios mudam

## Static vs Dynamic RLS

### Static RLS
Regras fixas por role:
```dax
// Role: West Region
[Region] = "West"

// Role: East Region  
[Region] = "East"
```

**Pros:** Simples, claro
**Cons:** Nao escala, exige role por grupo

### Dynamic RLS
Identidade do usuario direciona o filtro:
```dax
// Single role filters based on logged-in user
[ManagerEmail] = USERPRINCIPALNAME()
```

**Pros:** Escala, auto-maintaining
**Cons:** Exige dados de mapeamento de usuarios

## Padroes de Implementacao

### Padrao 1: Mapeamento Direto de Usuario
Email do usuario na tabela de dimensao:
```dax
// On Customer table
[CustomerEmail] = USERPRINCIPALNAME()
```

### Padrao 2: Tabela de Seguranca
Tabela separada mapeando usuarios para dados:
```
SecurityMapping table:
| UserEmail | Region |
|-----------|--------|
| joe@co.com | West  |
| sue@co.com | East  |
```

```dax
// On Region dimension
[Region] IN 
    SELECTCOLUMNS(
        FILTER(SecurityMapping, [UserEmail] = USERPRINCIPALNAME()),
        "Region", [Region]
    )
```

### Padrao 3: Hierarquia de Managers
Usuarios veem seus dados e os de subordinados:
```dax
// Using PATH functions for hierarchy
PATHCONTAINS(Employee[ManagerPath], 
    LOOKUPVALUE(Employee[EmployeeID], Employee[Email], USERPRINCIPALNAME()))
```

### Padrao 4: Multiplas Regras
Combine condicoes:
```dax
// Users see their region OR if they're a global viewer
[Region] = LOOKUPVALUE(Users[Region], Users[Email], USERPRINCIPALNAME())
|| LOOKUPVALUE(Users[IsGlobal], Users[Email], USERPRINCIPALNAME()) = TRUE()
```

## Criar Roles via MCP

### Listar Roles Existentes
```
security_role_operations(operation: "List")
```

### Criar Role com Permissao
```
security_role_operations(
  operation: "Create",
  definitions: [{
    name: "Regional Sales",
    modelPermission: "Read",
    description: "Restricts sales data by region"
  }]
)
```

### Adicionar Permissao de Tabela (Filtro)
```
security_role_operations(
  operation: "CreatePermissions",
  permissionDefinitions: [{
    roleName: "Regional Sales",
    tableName: "Customer",
    filterExpression: "[Region] = USERPRINCIPALNAME()"
  }]
)
```

### Obter Permissoes Efetivas
```
security_role_operations(
  operation: "GetEffectivePermissions",
  references: [{ name: "Regional Sales" }]
)
```

## Testar RLS

### No Power BI Desktop
1. Modeling tab > View As
2. Select role(s) to test
3. Optionally specify user identity
4. Verify data filtering

### Testar Valores Inesperados
Para dynamic RLS, teste:
- Usuarios validos
- Usuarios desconhecidos (devem ver nada ou falhar com elegancia)
- Valores NULL/blank

```dax
// Defensive pattern - returns no data for unknown users
IF(
    USERPRINCIPALNAME() IN VALUES(SecurityMapping[UserEmail]),
    [Region] IN SELECTCOLUMNS(...),
    FALSE()
)
```

## Erros Comuns

### 1. RLS Apenas em Fact Tables
**Problema:** Varreduras grandes de tabela, baixa performance
**Solucao:** Aplique em dimension tables, deixe os relacionamentos propagarem

### 2. Usar LOOKUPVALUE em vez de Relacionamentos
**Problema:** Caro, nao escala
**Solucao:** Crie relacionamentos corretos, deixe os filtros fluirem

### 3. Esperar Comportamento de Interseccao
**Problema:** Multiplas roles = UNION (aditivo), nao interseccao
**Solucao:** Projete roles considerando comportamento de uniao

### 4. Esquecer do DirectQuery
**Problema:** Filtros de RLS viram clausulas WHERE
**Solucao:** Garanta que o banco de origem suporte os padroes de query

### 5. Nao Testar Casos de Borda
**Problema:** Usuarios veem dados inesperados
**Solucao:** Teste com: usuarios validos, invalidos, multiplas roles

## RLS Bidirecional

Para relacionamentos bidirecionais com RLS:
```
Enable "Apply security filter in both directions"
```

Use somente quando:
- RLS requires filtering through many-to-many
- Dimension-to-dimension security needed

**Cuidado:** Apenas um relacionamento bidirecional por caminho e permitido.

## Consideracoes de Performance

- RLS adiciona clausulas WHERE a toda query
- DAX complexo em filtros prejudica a performance
- Teste com contagens realistas de usuarios
- Considere agregacoes para modelos grandes

## Object-Level Security (OLS)

Restringe acesso a tabelas ou colunas inteiras:
```
// Via XMLA/TMSL - not available in Desktop UI
```

Use para:
- Ocultar colunas sensiveis (salario, SSN)
- Restringir tabelas inteiras
- Combinado com RLS para seguranca completa

## Checklist de Validacao

- [ ] RLS aplicado em dimension tables (nao em fact tables)
- [ ] Filtros propagam corretamente pelos relacionamentos
- [ ] Dynamic RLS usa USERPRINCIPALNAME()
- [ ] Testado com usuarios validos e invalidos
- [ ] Casos de borda tratados (NULL, usuarios desconhecidos)
- [ ] Performance testada sob carga
- [ ] Mapeamentos de role documentados
- [ ] Roles de workspace compreendidas (Admins ignoram RLS)