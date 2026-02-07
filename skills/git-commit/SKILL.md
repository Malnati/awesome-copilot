---
name: git-commit
description: 'Execute git commit com analise de mensagem convencional, staging inteligente e geracao de mensagem. Use quando o usuario pedir para commitar mudancas, criar um git commit ou mencionar "/commit". Suporta: (1) Auto-detectar tipo e escopo a partir das mudancas, (2) Gerar mensagens Conventional Commits a partir do diff, (3) Commit interativo com overrides opcionais de tipo/escopo/descricao, (4) Staging inteligente de arquivos para agrupamento logico'
license: MIT
allowed-tools: Bash
---

# Git Commit com Conventional Commits

## Visao Geral

Crie commits git padronizados e semanticos usando a especificacao Conventional Commits. Analise o diff real para determinar tipo, escopo e mensagem apropriados.

## Formato de Conventional Commit

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Tipos de Commit

| Tipo       | Finalidade                     |
| ---------- | ------------------------------ |
| `feat`     | Nova feature                   |
| `fix`      | Bug fix                        |
| `docs`     | Apenas documentacao            |
| `style`    | Formatacao/estilo (sem logica) |
| `refactor` | Refatoracao (sem feature/fix)  |
| `perf`     | Melhoria de performance        |
| `test`     | Adiciona/atualiza testes       |
| `build`    | Build system/dependencies      |
| `ci`       | Mudancas de CI/config          |
| `chore`    | Manutencao/misc                |
| `revert`   | Reverter commit                |

## Breaking Changes

```
# Exclamation mark after type/scope
feat!: remove deprecated endpoint

# BREAKING CHANGE footer
feat: allow config to extend other configs

BREAKING CHANGE: `extends` key behavior changed
```

## Workflow

### 1. Analisar Diff

```bash
# If files are staged, use staged diff
git diff --staged

# If nothing staged, use working tree diff
git diff

# Also check status
git status --porcelain
```

### 2. Fazer Stage dos Arquivos (se necessario)

Se nada estiver em stage ou se voce quiser agrupar as mudancas de forma diferente:

```bash
# Stage specific files
git add path/to/file1 path/to/file2

# Stage by pattern
git add *.test.*
git add src/components/*

# Interactive staging
git add -p
```

**Nunca commite secrets** (.env, credentials.json, private keys).

### 3. Gerar Mensagem de Commit

Analise o diff para determinar:

- **Type**: Que tipo de mudanca e esta?
- **Scope**: Que area/modulo foi afetado?
- **Descricao**: Resumo em uma linha do que mudou (tempo presente, modo imperativo, <72 chars)

### 4. Executar Commit

```bash
# Single line
git commit -m "<type>[scope]: <description>"

# Multi-line with body/footer
git commit -m "$(cat <<'EOF'
<type>[scope]: <description>

<optional body>

<optional footer>
EOF
)"
```

## Boas Praticas

- Um commit por mudanca logica
- Tempo presente: "add" e nao "added"
- Modo imperativo: "fix bug" e nao "fixes bug"
- Referencie issues: `Closes #123`, `Refs #456`
- Mantenha a descricao abaixo de 72 caracteres

## Protocolo de Seguranca do Git

- NUNCA atualize git config
- NUNCA faca commit automaticamente com `--no-verify`
- NUNCA faca push sem confirmacao explicita do usuario
- NUNCA sobrescreva historico com `git rebase` ou `git push --force` sem confirmacao explicita
- NUNCA faca commit de segredos ou dados sensiveis
- Sempre pergunte antes de executar `git commit`
- Sempre mostre o resumo do commit antes de finalizar
