# GitHub Copilot SDK Cookbook

Este cookbook reune receitas pequenas e focadas que mostram como realizar tarefas comuns com o GitHub Copilot SDK em varias linguagens. Cada receita e propositalmente curta e pratica, com trechos prontos para copiar e colar e apontamentos para exemplos e testes mais completos.

## Receitas por Linguagem

### .NET (C#)

- [Error Handling](dotnet/error-handling.md): Lide com erros com elegancia, incluindo falhas de conexao, timeouts e limpeza.
- [Multiple Sessions](dotnet/multiple-sessions.md): Gerencie varias conversas independentes simultaneamente.
- [Managing Local Files](dotnet/managing-local-files.md): Organize arquivos por metadados usando estrategias de agrupamento com IA.
- [PR Visualization](dotnet/pr-visualization.md): Gere graficos interativos de idade de PR usando o GitHub MCP Server.
- [Persisting Sessions](dotnet/persisting-sessions.md): Salve e retome sessoes entre reinicios.

### Node.js / TypeScript

- [Error Handling](nodejs/error-handling.md): Lide com erros com elegancia, incluindo falhas de conexao, timeouts e limpeza.
- [Multiple Sessions](nodejs/multiple-sessions.md): Gerencie varias conversas independentes simultaneamente.
- [Managing Local Files](nodejs/managing-local-files.md): Organize arquivos por metadados usando estrategias de agrupamento com IA.
- [PR Visualization](nodejs/pr-visualization.md): Gere graficos interativos de idade de PR usando o GitHub MCP Server.
- [Persisting Sessions](nodejs/persisting-sessions.md): Salve e retome sessoes entre reinicios.

### Python

- [Error Handling](python/error-handling.md): Lide com erros com elegancia, incluindo falhas de conexao, timeouts e limpeza.
- [Multiple Sessions](python/multiple-sessions.md): Gerencie varias conversas independentes simultaneamente.
- [Managing Local Files](python/managing-local-files.md): Organize arquivos por metadados usando estrategias de agrupamento com IA.
- [PR Visualization](python/pr-visualization.md): Gere graficos interativos de idade de PR usando o GitHub MCP Server.
- [Persisting Sessions](python/persisting-sessions.md): Salve e retome sessoes entre reinicios.

### Go

- [Error Handling](go/error-handling.md): Lide com erros com elegancia, incluindo falhas de conexao, timeouts e limpeza.
- [Multiple Sessions](go/multiple-sessions.md): Gerencie varias conversas independentes simultaneamente.
- [Managing Local Files](go/managing-local-files.md): Organize arquivos por metadados usando estrategias de agrupamento com IA.
- [PR Visualization](go/pr-visualization.md): Gere graficos interativos de idade de PR usando o GitHub MCP Server.
- [Persisting Sessions](go/persisting-sessions.md): Salve e retome sessoes entre reinicios.

## Como Usar

- Navegue pela secao da sua linguagem acima e abra os links das receitas
- Cada receita inclui exemplos executaveis na subpasta `recipe/` com ferramentas especificas da linguagem
- Consulte exemplos e testes existentes como referencias:
  - Exemplos Node.js: `nodejs/examples/basic-example.ts`
  - Testes E2E: `go/e2e`, `python/e2e`, `nodejs/test/e2e`, `dotnet/test/Harness`

## Executando Exemplos

### .NET

```bash
cd dotnet/cookbook/recipe
dotnet run <filename>.cs
```

### Node.js

```bash
cd nodejs/cookbook/recipe
npm install
npx tsx <filename>.ts
```

### Python

```bash
cd python/cookbook/recipe
pip install -r requirements.txt
python <filename>.py
```

### Go

```bash
cd go/cookbook/recipe
go run <filename>.go
```

## Contribuindo

- Proponha ou adicione uma nova receita criando um arquivo markdown na pasta `cookbook/` da sua linguagem e um exemplo executavel em `recipe/`
- Siga as orientacoes do repositorio em [CONTRIBUTING.md](../../CONTRIBUTING.md)

## Status

A estrutura do cookbook esta completa com 4 receitas em todas as 4 linguagens suportadas. Cada receita inclui documentacao em markdown e exemplos executaveis.
