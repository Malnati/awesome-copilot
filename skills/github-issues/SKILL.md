---
name: github-issues
description: 'Crie, atualize e gerencie GitHub issues usando tools MCP. Use esta skill quando usuarios quiserem criar bug reports, feature requests ou task issues, atualizar issues existentes, adicionar labels/assignees/milestones ou gerenciar workflows de issues. Dispara em solicitacoes como "create an issue", "file a bug", "request a feature", "update issue X" ou qualquer tarefa de gerenciamento de GitHub issues.'
---

# Issues do GitHub

Gerencie GitHub issues usando o servidor MCP `@modelcontextprotocol/server-github`.

## Ferramentas MCP Disponiveis (Tools)

| Tool | Finalidade |
|------|------------|
| `mcp__github__create_issue` | Criar novas issues |
| `mcp__github__update_issue` | Atualizar issues existentes |
| `mcp__github__get_issue` | Obter detalhes da issue |
| `mcp__github__search_issues` | Buscar issues |
| `mcp__github__add_issue_comment` | Adicionar comentarios |
| `mcp__github__list_issues` | Listar issues do repositorio |

## Fluxo de Trabalho

1. **Determinar acao**: Criar, atualizar ou consultar?
2. **Coletar contexto**: Obter informacoes do repo, labels existentes, milestones se necessario
3. **Estruturar conteudo**: Use o template apropriado em [references/templates.md](references/templates.md)
4. **Executar**: Chame a tool MCP apropriada
5. **Confirmar**: Reporte a URL da issue ao usuario

## Criacao de Issues

### Parametros Obrigatorios

```
owner: dono do repositorio (org ou usuario)
repo: nome do repositorio  
title: titulo claro e acionavel
body: conteudo markdown estruturado
```

### Parametros Opcionais

```
labels: ["bug", "enhancement", "documentation", ...]
assignees: ["username1", "username2"]
milestone: numero do milestone (integer)
```

### Diretrizes de Titulo

- Comece com prefixo de tipo quando util: `[Bug]`, `[Feature]`, `[Docs]`
- Seja especifico e acionavel
- Mantenha abaixo de 72 caracteres
- Exemplos:
  - `[Bug] Login fails with SSO enabled`
  - `[Feature] Add dark mode support`
  - `Add unit tests for auth module`

### Estrutura do Body

Sempre use os templates em [references/templates.md](references/templates.md). Escolha com base no tipo de issue:

| Solicitacao do Usuario | Template |
|--------------|----------|
| Bug, error, broken, not working | Bug Report |
| Feature, enhancement, add, new | Feature Request |
| Task, chore, refactor, update | Task |

## Atualizando Issues

Use `mcp__github__update_issue` com:

```
owner, repo, issue_number (required)
title, body, state, labels, assignees, milestone (optional - only changed fields)
```

Valores de state: `open`, `closed`

## Exemplos

### Exemplo 1: Bug Report

**Usuario**: "Crie um bug issue - a pagina de login quebra ao usar SSO"

**Action**: Chame `mcp__github__create_issue` com:
```json
{
  "owner": "github",
  "repo": "awesome-copilot",
  "title": "[Bug] Login page crashes when using SSO",
  "body": "## Description\nThe login page crashes when users attempt to authenticate using SSO.\n\n## Steps to Reproduce\n1. Navigate to login page\n2. Click 'Sign in with SSO'\n3. Page crashes\n\n## Expected Behavior\nSSO authentication should complete and redirect to dashboard.\n\n## Actual Behavior\nPage becomes unresponsive and displays error.\n\n## Environment\n- Browser: [To be filled]\n- OS: [To be filled]\n\n## Additional Context\nReported by user.",
  "labels": ["bug"]
}
```

### Exemplo 2: Feature Request

**Usuario**: "Crie um feature request para dark mode com alta prioridade"

**Action**: Chame `mcp__github__create_issue` com:
```json
{
  "owner": "github",
  "repo": "awesome-copilot",
  "title": "[Feature] Add dark mode support",
  "body": "## Summary\nAdd dark mode theme option for improved user experience and accessibility.\n\n## Motivation\n- Reduces eye strain in low-light environments\n- Increasingly expected by users\n- Improves accessibility\n\n## Proposed Solution\nImplement theme toggle with system preference detection.\n\n## Acceptance Criteria\n- [ ] Toggle switch in settings\n- [ ] Persists user preference\n- [ ] Respects system preference by default\n- [ ] All UI components support both themes\n\n## Alternatives Considered\nNone specified.\n\n## Additional Context\nHigh priority request.",
  "labels": ["enhancement", "high-priority"]
}
```

## Labels Comuns

Use estes labels padrao quando aplicavel:

| Label | Uso |
|-------|-----|
| `bug` | Algo nao esta funcionando |
| `enhancement` | Nova feature ou melhoria |
| `documentation` | Atualizacoes de documentacao |
| `good first issue` | Bom para iniciantes |
