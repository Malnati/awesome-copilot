---
name: snowflake-semanticview
description: Crie, altere e valide semantic views do Snowflake usando Snowflake CLI (snow). Use ao solicitar construir ou solucionar semantic views/definicoes da camada semantica com CREATE/ALTER SEMANTIC VIEW, para validar DDL de semantic view no Snowflake via CLI, ou para guiar instalacao do Snowflake CLI e setup de conexao.
---

# Snowflake Semantic Views

## Configuracao Unica

- Verifique a instalacao do Snowflake CLI abrindo um novo terminal e executando `snow --help`.
- Se o Snowflake CLI estiver faltando ou o usuario nao puder instalar, direcione para https://docs.snowflake.com/en/developer-guide/snowflake-cli/installation/installation.
- Configure uma conexao Snowflake com `snow connection add` conforme https://docs.snowflake.com/en/developer-guide/snowflake-cli/connecting/configure-connections#add-a-connection.
- Use a conexao configurada para todos os passos de validacao e execucao.

## Workflow para Cada Solicitacao de Semantic View

1. Confirme database, schema, role, warehouse e o nome final da semantic view.
2. Confirme que o modelo segue star schema (fatos com dimensoes conformadas).
3. Redija o DDL da semantic view usando a sintaxe oficial:
   - https://docs.snowflake.com/en/sql-reference/sql/create-semantic-view
4. Preencha synonyms e comments para cada dimensao, fato e metrica:
   - Leia comentarios de tabelas/views/colunas do Snowflake primeiro (fonte preferida):
     - https://docs.snowflake.com/en/sql-reference/sql/comment
   - Se comments ou synonyms estiverem ausentes, pergunte se pode cria-los, se o usuario quer fornecer texto, ou se voce deve rascunhar sugestoes para aprovacao.
5. Use SELECT com DISTINCT e LIMIT (maximo 1000 linhas) para descobrir relacionamentos entre tabelas fato e dimensao, identificar tipos de coluna e criar comments e synonyms mais significativos para colunas.
6. Crie um nome temporario de validacao (por exemplo, adicionando `__tmp_validate`) mantendo o mesmo database e schema.
7. Sempre valide enviando o DDL para o Snowflake via Snowflake CLI antes de finalizar:
   - Use `snow sql` para executar o statement com a conexao configurada.
   - Se flags diferirem por versao, cheque `snow sql --help` e use a opcao de conexao exibida.
8. Se a validacao falhar, itere no DDL e reexecute a validacao ate passar.
9. Aplique o DDL final (create ou alter) usando o nome real da semantic view.
10. Rode uma query de exemplo na semantic view final para confirmar funcionamento. Ela tem sintaxe SQL diferente conforme: https://docs.snowflake.com/en/user-guide/views-semantic/querying#querying-a-semantic-view
Exemplo:

```SQL
SELECT * FROM SEMANTIC_VIEW(
    my_semview_name
    DIMENSIONS customer.customer_market_segment
    METRICS orders.order_average_value
)
ORDER BY customer_market_segment;
```

11. Limpe qualquer semantic view temporaria criada durante a validacao.

## Synonyms e Comments (Obrigatorio)

- Use a sintaxe de semantic view para synonyms e comments:

```
WITH SYNONYMS [ = ] ( 'synonym' [ , ... ] )
COMMENT = 'comment_about_dim_fact_or_metric'
```

- Trate synonyms apenas como informativos; nao use para referenciar dimensoes, fatos ou metricas em outros lugares.
- Use comentarios do Snowflake como fonte preferida e primeira para synonyms e comments:
  - https://docs.snowflake.com/en/sql-reference/sql/comment
- Se comentarios do Snowflake estiverem faltando, pergunte se pode cria-los, se o usuario quer fornecer texto, ou se voce deve rascunhar sugestoes para aprovacao.
- Nao invente synonyms ou comments sem aprovacao do usuario.

## Padrao de Validacao (Obrigatorio)

- Nunca pule a validacao. Sempre execute o DDL no Snowflake via Snowflake CLI antes de apresentar como final.
- Prefira um nome temporario para validacao para evitar sobrescrever a view real.

## Exemplo de Validacao via CLI (Template)

```bash
# Replace placeholders with real values.
snow sql -q "<CREATE OR ALTER SEMANTIC VIEW ...>" --connection <connection_name>
```

Se o CLI usar um flag de conexao diferente na sua versao, rode:

```bash
snow sql --help
```

## Notas

- Trate instalacao e setup de conexao como passos unicos, mas confirme que foram feitos antes da primeira validacao.
- Mantenha a definicao final identica a definicao temporaria validada, exceto pelo nome.
- Nao omita synonyms ou comments; considere-os obrigatorios para completude mesmo que opcionais na sintaxe.
