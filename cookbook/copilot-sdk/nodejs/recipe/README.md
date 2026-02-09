# Exemplos de Receita Executaveis

Esta pasta contem exemplos TypeScript independentes e executaveis para cada receita do cookbook. Cada arquivo pode ser executado diretamente com `tsx` ou via scripts npm.

## Pre-requisitos

- Node.js 18 ou superior
- Instale as dependencias (isso referencia o SDK local no repo):

```bash
npm install
```

## Executando Exemplos

Cada arquivo `.ts` e um programa completo e executavel. Voce pode executa-los de duas formas:

### Usando scripts npm:

```bash
npm run <script-name>
```

### Usando tsx diretamente:

```bash
npx tsx <filename>.ts
```

### Receitas Disponiveis

| Receita               | npm script                     | Comando direto                    | Descricao                                  |
| --------------------- | ------------------------------ | --------------------------------- | ------------------------------------------ |
| Error Handling        | `npm run error-handling`       | `npx tsx error-handling.ts`       | Demonstra padroes de tratamento de erros   |
| Multiple Sessions     | `npm run multiple-sessions`    | `npx tsx multiple-sessions.ts`    | Gerencia multiplas conversas independentes |
| Managing Local Files  | `npm run managing-local-files` | `npx tsx managing-local-files.ts` | Organiza arquivos com agrupamento por IA   |
| PR Visualization      | `npm run pr-visualization`     | `npx tsx pr-visualization.ts`     | Gera graficos de idade de PR               |
| Persisting Sessions   | `npm run persisting-sessions`  | `npx tsx persisting-sessions.ts`  | Salva e retoma sessoes entre reinicios     |

### Exemplos com Argumentos

**PR Visualization com repo especifico:**

```bash
npx tsx pr-visualization.ts --repo github/copilot-sdk
```

**Managing Local Files (edite o arquivo para mudar a pasta alvo):**

```bash
# Edit the targetFolder variable in managing-local-files.ts first
npx tsx managing-local-files.ts
```

## Desenvolvimento do SDK Local

O `package.json` referencia o Copilot SDK local usando `"*"`, que resolve para a fonte local do SDK. Isso significa:

- Mudancas no codigo do SDK ficam disponiveis imediatamente
- Nao ha necessidade de publicar ou instalar do npm
- Perfeito para testes e desenvolvimento

Se voce modificar o codigo do SDK, talvez precise reconstruir:

```bash
cd ../../src
npm run build
```

## Recursos do TypeScript

Estes exemplos usam recursos modernos de TypeScript/Node.js:

- Top-level await (requer `"type": "module"` no package.json)
- Imports ESM
- Seguranca de tipos com TypeScript
- Padroes async/await

## Recursos de Aprendizado

- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [Node.js Documentation](https://nodejs.org/docs/latest/api/)
- [GitHub Copilot SDK for Node.js](https://github.com/github/copilot-sdk/blob/main/nodejs/README.md)
- [Cookbook Pai](../README.md)
