# Agrupando Arquivos por Metadados

Use o Copilot para organizar arquivos de forma inteligente em uma pasta com base em seus metadados.

> **Exemplo executavel:** [recipe/managing-local-files.ts](recipe/managing-local-files.ts)
>
> ```bash
> cd recipe && npm install
> npx tsx managing-local-files.ts
> # or: npm run managing-local-files
> ```

## Cenario de exemplo

Voce tem uma pasta com muitos arquivos e quer organiza-los em subpastas com base em metadados como tipo de arquivo, data de criacao, tamanho ou outros atributos. O Copilot pode analisar os arquivos e sugerir ou executar uma estrategia de agrupamento.

## Codigo de exemplo

```typescript
import { CopilotClient } from "@github/copilot-sdk";
import * as os from "node:os";
import * as path from "node:path";

// Create and start client
const client = new CopilotClient();
await client.start();

// Create session
const session = await client.createSession({
    model: "gpt-5",
});

// Event handler
session.on((event) => {
    switch (event.type) {
        case "assistant.message":
            console.log(`\nCopilot: ${event.data.content}`);
            break;
        case "tool.execution_start":
            console.log(`  → Running: ${event.data.toolName} ${event.data.toolCallId}`);
            break;
        case "tool.execution_complete":
            console.log(`  ✓ Completed: ${event.data.toolCallId}`);
            break;
    }
});

// Ask Copilot to organize files
const targetFolder = path.join(os.homedir(), "Downloads");

await session.sendAndWait({
    prompt: `
Analyze the files in "${targetFolder}" and organize them into subfolders.

1. First, list all files and their metadata
2. Preview grouping by file extension
3. Create appropriate subfolders (e.g., "images", "documents", "videos")
4. Move each file to its appropriate subfolder

Please confirm before moving any files.
`,
});

await session.destroy();
await client.stop();
```

## Estrategias de agrupamento

### Por extensao de arquivo

```typescript
// Groups files like:
// images/   -> .jpg, .png, .gif
// documents/ -> .pdf, .docx, .txt
// videos/   -> .mp4, .avi, .mov
```

### Por data de criacao

```typescript
// Groups files like:
// 2024-01/ -> files created in January 2024
// 2024-02/ -> files created in February 2024
```

### Por tamanho de arquivo

```typescript
// Groups files like:
// tiny-under-1kb/
// small-under-1mb/
// medium-under-100mb/
// large-over-100mb/
```

## Modo dry-run

Por seguranca, voce pode pedir ao Copilot para apenas prever as alteracoes:

```typescript
await session.sendAndWait({
    prompt: `
Analyze files in "${targetFolder}" and show me how you would organize them
by file type. DO NOT move any files - just show me the plan.
`,
});
```

## Agrupamento personalizado com analise de IA

Deixe o Copilot determinar o melhor agrupamento com base no conteudo dos arquivos:

```typescript
await session.sendAndWait({
    prompt: `
Look at the files in "${targetFolder}" and suggest a logical organization.
Consider:
- File names and what they might contain
- File types and their typical uses
- Date patterns that might indicate projects or events

Propose folder names that are descriptive and useful.
`,
});
```

## Consideracoes de seguranca

1. **Confirme antes de mover**: Peça ao Copilot para confirmar antes de executar movimentacoes
2. **Trate duplicatas**: Considere o que acontece se existir um arquivo com o mesmo nome
3. **Preserve os originais**: Considere copiar em vez de mover para arquivos importantes
