---
description: "Prompt para gerar um arquivo AGENTS.md para um repositorio"
agent: "agent"
---

# Criar arquivo AGENTS.md de alta qualidade

Voce e um code agent. Sua tarefa e criar um AGENTS.md completo e preciso na raiz deste repositorio que siga a orientacao publica em https://agents.md/.

AGENTS.md e um formato aberto projetado para fornecer a agentes de codigo o contexto e as instrucoes necessarias para trabalhar efetivamente em um projeto.

## O que e AGENTS.md?

AGENTS.md e um arquivo Markdown que serve como um "README para agentes" - um local dedicado e previsivel para fornecer contexto e instrucoes para ajudar agentes de codigo de IA a trabalhar no projeto. Ele complementa o README.md ao conter contexto tecnico detalhado que agentes precisam, mas que poderia poluir um README voltado a humanos.

## Principios-Chave

- **Focado em agentes**: Contem instrucoes tecnicas detalhadas para ferramentas automatizadas
- **Complementa README.md**: Nao substitui documentacao humana, mas adiciona contexto especifico para agentes
- **Local padronizado**: Colocado na raiz do repositorio (ou em subprojetos em monorepos)
- **Formato aberto**: Usa Markdown padrao com estrutura flexivel
- **Compatibilidade no ecossistema**: Funciona com 20+ ferramentas e agentes de IA

## Estrutura do Arquivo e Diretrizes de Conteudo

### 1. Setup Obrigatorio

- Crie o arquivo como `AGENTS.md` na raiz do repositorio
- Use formatacao Markdown padrao
- Sem campos obrigatorios - estrutura flexivel baseada nas necessidades do projeto

### 2. Secoes Essenciais a Incluir

#### Project Overview

- Breve descricao do que o projeto faz
- Visao geral de arquitetura se for complexa
- Principais tecnologias e frameworks usados

#### Setup Commands

- Instrucoes de instalacao
- Etapas de setup do ambiente
- Comandos de gerenciamento de dependencias
- Setup de banco de dados se aplicavel

#### Development Workflow

- Como iniciar o servidor de desenvolvimento
- Comandos de build
- Setup de watch/hot-reload
- Especificos do gerenciador de pacotes (npm, pnpm, yarn, etc.)

#### Testing Instructions

- Como rodar testes (unit, integration, e2e)
- Localizacao e convencoes de nome de testes
- Requisitos de cobertura
- Frameworks ou patterns de teste usados
- Como rodar subconjuntos de testes ou focar areas especificas

#### Code Style Guidelines

- Convencoes especificas da linguagem
- Regras de linting e formatting
- Padroes de organizacao de arquivos
- Convencoes de nome
- Padroes de import/export

#### Build and Deployment

- Comandos e outputs de build
- Configuracoes de ambiente
- Etapas e requisitos de deploy
- Informacoes de pipeline CI/CD

### 3. Secoes Opcionais mas Recomendadas

#### Security Considerations

- Requisitos de testes de seguranca
- Gestao de secrets
- Padroes de autenticacao
- Modelos de permissao

#### Instrucoes de Monorepo (se aplicavel)

- Como trabalhar com multiplos pacotes
- Dependencias entre pacotes
- Build/test seletivo
- Comandos especificos por pacote

#### Pull Request Guidelines

- Requisitos de formato de titulo
- Checks obrigatorios antes do envio
- Processo de review
- Convencoes de mensagem de commit

#### Debugging and Troubleshooting

- Problemas comuns e solucoes
- Padroes de logging
- Configuracao de debug
- Consideracoes de performance

## Template de Exemplo

Use este template como ponto de partida e customize conforme o projeto:

```markdown
# AGENTS.md

## Project Overview

[Brief description of the project, its purpose, and key technologies]

## Setup Commands

- Install dependencies: `[package manager] install`
- Start development server: `[command]`
- Build for production: `[command]`

## Development Workflow

- [Development server startup instructions]
- [Hot reload/watch mode information]
- [Environment variable setup]

## Testing Instructions

- Run all tests: `[command]`
- Run unit tests: `[command]`
- Run integration tests: `[command]`
- Test coverage: `[command]`
- [Specific testing patterns or requirements]

## Code Style

- [Language and framework conventions]
- [Linting rules and commands]
- [Formatting requirements]
- [File organization patterns]

## Build and Deployment

- [Build process details]
- [Output directories]
- [Environment-specific builds]
- [Deployment commands]

## Pull Request Guidelines

- Title format: [component] Brief description
- Required checks: `[lint command]`, `[test command]`
- [Review requirements]

## Additional Notes

- [Any project-specific context]
- [Common gotchas or troubleshooting tips]
- [Performance considerations]
```

## Exemplo Real do agents.md

Aqui esta um exemplo real do site agents.md:

```markdown
# Sample AGENTS.md file

## Dev environment tips

- Use `pnpm dlx turbo run where <project_name>` to jump to a package instead of scanning with `ls`.
- Run `pnpm install --filter <project_name>` to add the package to your workspace so Vite, ESLint, and TypeScript can see it.
- Use `pnpm create vite@latest <project_name> -- --template react-ts` to spin up a new React + Vite package with TypeScript checks ready.
- Check the name field inside each package's package.json to confirm the right name—skip the top-level one.

## Testing instructions

- Find the CI plan in the .github/workflows folder.
- Run `pnpm turbo run test --filter <project_name>` to run every check defined for that package.
- From the package root you can just call `pnpm test`. The commit should pass all tests before you merge.
- To focus on one step, add the Vitest pattern: `pnpm vitest run -t "<test name>"`.
- Fix any test or type errors until the whole suite is green.
- After moving files or changing imports, run `pnpm lint --filter <project_name>` to be sure ESLint and TypeScript rules still pass.
- Add or update tests for the code you change, even if nobody asked.

## PR instructions

- Title format: [<project_name>] <Title>
- Always run `pnpm lint` and `pnpm test` before committing.
```

## Etapas de Implementacao

1. **Analise a estrutura do projeto** para entender:

   - Linguagens e frameworks usados
   - Gerenciadores de pacotes e build tools
   - Frameworks de teste
   - Arquitetura do projeto (monorepo, single package, etc.)

2. **Identifique workflows-chave** examinando:

   - package.json scripts
   - Makefile ou outros arquivos de build
   - Arquivos de CI/CD
   - Arquivos de documentacao

3. **Crie secoes abrangentes** cobrindo:

   - Todos os comandos essenciais de setup e desenvolvimento
   - Estrategias e comandos de teste
   - Code style e convencoes
   - Processos de build e deployment

4. **Inclua comandos especificos e acionaveis** que agentes possam executar diretamente

5. **Teste as instrucoes** garantindo que todos os comandos funcionem conforme documentado

6. **Mantenha o foco** no que agentes precisam saber, nao em informacao geral do projeto

## Best Practices

- **Seja especifico**: Inclua comandos exatos, nao descricoes vagas
- **Use code blocks**: Envolva comandos em backticks para clareza
- **Inclua contexto**: Explique por que certas etapas sao necessarias
- **Mantenha atual**: Atualize conforme o projeto evolui
- **Teste comandos**: Garanta que todos os comandos listados funcionem
- **Considere arquivos aninhados**: Para monorepos, crie AGENTS.md em subprojetos conforme necessario

## Consideracoes de Monorepo

Para monorepos grandes:

- Coloque um AGENTS.md principal na raiz
- Crie AGENTS.md adicionais em diretorios de subprojetos
- O AGENTS.md mais proximo tem precedencia para qualquer localizacao
- Inclua dicas de navegacao entre pacotes/projetos

## Notas Finais

- AGENTS.md funciona com 20+ ferramentas de IA, incluindo Cursor, Aider, Gemini CLI e outras
- O formato e intencionalmente flexivel - adapte as necessidades do projeto
- Foque em instrucoes acionaveis que ajudem agentes a entender e trabalhar com o codebase
- Esta e documentacao viva - atualize conforme o projeto evolui

Ao criar o AGENTS.md, priorize clareza, completude e acionabilidade. O objetivo e dar a qualquer agente contexto suficiente para contribuir efetivamente sem exigir orientacao humana adicional.
