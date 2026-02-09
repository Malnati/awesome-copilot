---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Assistente universal de code review SQL que faz analise abrangente de seguranca, manutenibilidade e qualidade de codigo em todos os bancos SQL (MySQL, PostgreSQL, SQL Server, Oracle). Foca em prevencao de SQL injection, controle de acesso, padroes de codigo e deteccao de anti-padroes. Complementa o prompt de otimizacao SQL para cobertura completa de desenvolvimento.'
tested_with: 'GitHub Copilot Chat (GPT-4o) - Validated July 20, 2025'
---

# SQL Code Review

Execute um code review SQL completo de ${selection} (ou do projeto inteiro se nao houver selecao) focando em seguranca, performance, manutenibilidade e boas praticas de banco.

## 🔒 Analise de Seguranca

### SQL Injection Prevention
```sql
-- ❌ CRITICAL: SQL Injection vulnerability
query = "SELECT * FROM users WHERE id = " + userInput;
query = f"DELETE FROM orders WHERE user_id = {user_id}";

-- ✅ SECURE: Parameterized queries
-- PostgreSQL/MySQL
PREPARE stmt FROM 'SELECT * FROM users WHERE id = ?';
EXECUTE stmt USING @user_id;

-- SQL Server
EXEC sp_executesql N'SELECT * FROM users WHERE id = @id', N'@id INT', @id = @user_id;
```

### Access Control & Permissions
- **Principle of Least Privilege**: Conceda o minimo de permissoes necessario
- **Role-Based Access**: Use roles em vez de permissoes diretas por usuario
- **Schema Security**: Propriedade de schema e controles de acesso apropriados
- **Function/Procedure Security**: Revise direitos DEFINER vs INVOKER

### Data Protection
- **Sensitive Data Exposure**: Evite SELECT * em tabelas com colunas sensiveis
- **Audit Logging**: Garanta que operacoes sensiveis sejam logadas
- **Data Masking**: Use views ou functions para mascarar dados sensiveis
- **Encryption**: Verifique armazenamento criptografado para dados sensiveis

## ⚡ Performance Optimization

### Query Structure Analysis
```sql
-- ❌ BAD: Inefficient query patterns
SELECT DISTINCT u.* 
FROM users u, orders o, products p
WHERE u.id = o.user_id 
AND o.product_id = p.id
AND YEAR(o.order_date) = 2024;

-- ✅ GOOD: Optimized structure
SELECT u.id, u.name, u.email
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.order_date >= '2024-01-01' 
AND o.order_date < '2025-01-01';
```

### Index Strategy Review
- **Missing Indexes**: Identifique colunas que precisam de indice
- **Over-Indexing**: Encontre indices nao usados ou redundantes
- **Composite Indexes**: Indices multi-coluna para queries complexas
- **Index Maintenance**: Verifique indices fragmentados ou desatualizados

### Join Optimization
- **Join Types**: Verifique tipos adequados (INNER vs LEFT vs EXISTS)
- **Join Order**: Otimize para menores result sets primeiro
- **Cartesian Products**: Identifique e corrija falta de condicoes de join
- **Subquery vs JOIN**: Escolha a abordagem mais eficiente

### Aggregate and Window Functions
```sql
-- ❌ BAD: Inefficient aggregation
SELECT user_id, 
       (SELECT COUNT(*) FROM orders o2 WHERE o2.user_id = o1.user_id) as order_count
FROM orders o1
GROUP BY user_id;

-- ✅ GOOD: Efficient aggregation
SELECT user_id, COUNT(*) as order_count
FROM orders
GROUP BY user_id;
```

## 🛠️ Code Quality & Maintainability

### SQL Style & Formatting
```sql
-- ❌ BAD: Poor formatting and style
select u.id,u.name,o.total from users u left join orders o on u.id=o.user_id where u.status='active' and o.order_date>='2024-01-01';

-- ✅ GOOD: Clean, readable formatting
SELECT u.id,
       u.name,
       o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.status = 'active'
  AND o.order_date >= '2024-01-01';
```

### Naming Conventions
- **Consistent Naming**: Tabelas, colunas e constraints seguem padroes consistentes
- **Descriptive Names**: Nomes claros e significativos para objetos do banco
- **Reserved Words**: Evite palavras reservadas como identificadores
- **Case Sensitivity**: Uso consistente de caixa em todo o schema

### Schema Design Review
- **Normalization**: Nivel apropriado de normalizacao (evite excesso/falta)
- **Data Types**: Escolha otima de tipos para storage e performance
- **Constraints**: Uso adequado de PRIMARY KEY, FOREIGN KEY, CHECK, NOT NULL
- **Default Values**: Valores default apropriados para colunas

## 🗄️ Database-Specific Best Practices

### PostgreSQL
```sql
-- Use JSONB for JSON data
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    data JSONB NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- GIN index for JSONB queries
CREATE INDEX idx_events_data ON events USING gin(data);

-- Array types for multi-value columns
CREATE TABLE tags (
    post_id INT,
    tag_names TEXT[]
);
```

### MySQL
```sql
-- Use appropriate storage engines
CREATE TABLE sessions (
    id VARCHAR(128) PRIMARY KEY,
    data TEXT,
    expires TIMESTAMP
) ENGINE=InnoDB;

-- Optimize for InnoDB
ALTER TABLE large_table 
ADD INDEX idx_covering (status, created_at, id);
```

### SQL Server
```sql
-- Use appropriate data types
CREATE TABLE products (
    id BIGINT IDENTITY(1,1) PRIMARY KEY,
    name NVARCHAR(255) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    created_at DATETIME2 DEFAULT GETUTCDATE()
);

-- Columnstore indexes for analytics
CREATE COLUMNSTORE INDEX idx_sales_cs ON sales;
```

### Oracle
```sql
-- Use sequences for auto-increment
CREATE SEQUENCE user_id_seq START WITH 1 INCREMENT BY 1;

CREATE TABLE users (
    id NUMBER DEFAULT user_id_seq.NEXTVAL PRIMARY KEY,
    name VARCHAR2(255) NOT NULL
);
```

## 🧪 Testing & Validation

### Data Integrity Checks
```sql
-- Verify referential integrity
SELECT o.user_id 
FROM orders o 
LEFT JOIN users u ON o.user_id = u.id 
WHERE u.id IS NULL;

-- Check for data consistency
SELECT COUNT(*) as inconsistent_records
FROM products 
WHERE price < 0 OR stock_quantity < 0;
```

### Performance Testing
- **Execution Plans**: Revise planos de execucao
- **Load Testing**: Teste queries com volumes realistas
- **Stress Testing**: Verifique performance com carga concorrente
- **Regression Testing**: Garanta que otimizacoes nao quebrem funcionalidade

## 📊 Common Anti-Patterns

### N+1 Query Problem
```sql
-- ❌ BAD: N+1 queries in application code
for user in users:
    orders = query("SELECT * FROM orders WHERE user_id = ?", user.id)

-- ✅ GOOD: Single optimized query
SELECT u.*, o.*
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

### Overuse of DISTINCT
```sql
-- ❌ BAD: DISTINCT masking join issues
SELECT DISTINCT u.name 
FROM users u, orders o 
WHERE u.id = o.user_id;

-- ✅ GOOD: Proper join without DISTINCT
SELECT u.name
FROM users u
INNER JOIN orders o ON u.id = o.user_id
GROUP BY u.name;
```

### Function Misuse in WHERE Clauses
```sql
-- ❌ BAD: Functions prevent index usage
SELECT * FROM orders 
WHERE YEAR(order_date) = 2024;

-- ✅ GOOD: Range conditions use indexes
SELECT * FROM orders 
WHERE order_date >= '2024-01-01' 
  AND order_date < '2025-01-01';
```

## 📋 SQL Review Checklist

### Security
- [ ] All user inputs are parameterized
- [ ] No dynamic SQL construction with string concatenation
- [ ] Appropriate access controls and permissions
- [ ] Sensitive data is properly protected
- [ ] SQL injection attack vectors are eliminated

### Performance
- [ ] Indexes exist for frequently queried columns
- [ ] No unnecessary SELECT * statements
- [ ] JOINs are optimized and use appropriate types
- [ ] WHERE clauses are selective and use indexes
- [ ] Subqueries are optimized or converted to JOINs

### Code Quality
- [ ] Consistent naming conventions
- [ ] Proper formatting and indentation
- [ ] Meaningful comments for complex logic
- [ ] Appropriate data types are used
- [ ] Error handling is implemented

### Schema Design
- [ ] Tables are properly normalized
- [ ] Constraints enforce data integrity
- [ ] Indexes support query patterns
- [ ] Foreign key relationships are defined
- [ ] Default values are appropriate

## 🎯 Review Output Format

### Issue Template
```
## [PRIORITY] [CATEGORY]: [Brief Description]

**Location**: [Table/View/Procedure name and line number if applicable]
**Issue**: [Detailed explanation of the problem]
**Security Risk**: [If applicable - injection risk, data exposure, etc.]
**Performance Impact**: [Query cost, execution time impact]
**Recommendation**: [Specific fix with code example]

**Before**:
```sql
-- Problematic SQL
```

**After**:
```sql
-- Improved SQL
```

**Expected Improvement**: [Performance gain, security benefit]
```

### Summary Assessment
- **Security Score**: [1-10] - SQL injection protection, access controls
- **Performance Score**: [1-10] - Query efficiency, index usage
- **Maintainability Score**: [1-10] - Code quality, documentation
- **Schema Quality Score**: [1-10] - Design patterns, normalization

### Top 3 Priority Actions
1. **[Critical Security Fix]**: Address SQL injection vulnerabilities
2. **[Performance Optimization]**: Add missing indexes or optimize queries
3. **[Code Quality]**: Improve naming conventions and documentation

Foque em fornecer recomendacoes acionaveis e agnosticas de banco, destacando otimizacoes e boas praticas especificas de plataforma.
