# Padroes de Tratamento de Erros

Trate erros com elegancia em suas aplicacoes do Copilot SDK.

> **Exemplo executavel:** [recipe/error-handling.ts](recipe/error-handling.ts)
>
> ```bash
> cd recipe && npm install
> npx tsx error-handling.ts
> # or: npm run error-handling
> ```

## Cenario de exemplo

Voce precisa lidar com varias condicoes de erro como falhas de conexao, timeouts e respostas invalidas.

## Try-catch basico

```typescript
import { CopilotClient } from "@github/copilot-sdk";

const client = new CopilotClient();

try {
    await client.start();
    const session = await client.createSession({ model: "gpt-5" });

    const response = await session.sendAndWait({ prompt: "Hello!" });
    console.log(response?.data.content);

    await session.destroy();
} catch (error) {
    console.error("Error:", error.message);
} finally {
    await client.stop();
}
```

## Tratando tipos de erro especificos

```typescript
try {
    await client.start();
} catch (error) {
    if (error.message.includes("ENOENT")) {
        console.error("Copilot CLI not found. Please install it first.");
    } else if (error.message.includes("ECONNREFUSED")) {
        console.error("Could not connect to Copilot CLI server.");
    } else {
        console.error("Unexpected error:", error.message);
    }
}
```

## Tratamento de timeout

```typescript
const session = await client.createSession({ model: "gpt-5" });

try {
    // sendAndWait with timeout (in milliseconds)
    const response = await session.sendAndWait(
        { prompt: "Complex question..." },
        30000 // 30 second timeout
    );

    if (response) {
        console.log(response.data.content);
    } else {
        console.log("No response received");
    }
} catch (error) {
    if (error.message.includes("timeout")) {
        console.error("Request timed out");
    }
}
```

## Abortando uma requisicao

```typescript
const session = await client.createSession({ model: "gpt-5" });

// Start a request
session.send({ prompt: "Write a very long story..." });

// Abort it after some condition
setTimeout(async () => {
    await session.abort();
    console.log("Request aborted");
}, 5000);
```

## Encerramento elegante

```typescript
process.on("SIGINT", async () => {
    console.log("Shutting down...");

    const errors = await client.stop();
    if (errors.length > 0) {
        console.error("Cleanup errors:", errors);
    }

    process.exit(0);
});
```

## Parada forcada

```typescript
// If stop() takes too long, force stop
const stopPromise = client.stop();
const timeout = new Promise((_, reject) => setTimeout(() => reject(new Error("Timeout")), 5000));

try {
    await Promise.race([stopPromise, timeout]);
} catch {
    console.log("Forcing stop...");
    await client.forceStop();
}
```

## Melhores praticas

1. **Sempre limpe**: Use try-finally para garantir que `client.stop()` seja chamado
2. **Trate erros de conexao**: A CLI pode nao estar instalada ou em execucao
3. **Defina timeouts apropriados**: Requisicoes longas devem ter timeouts
4. **Registre erros**: Capture detalhes de erros para depuracao
