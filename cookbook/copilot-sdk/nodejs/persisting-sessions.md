# Persistencia e Retomada de Sessoes

Salve e restaure sessoes de conversa entre reinicios da aplicacao.

## Cenario de exemplo

Voce quer que os usuarios possam continuar uma conversa mesmo depois de fechar e reabrir seu aplicativo.

> **Exemplo executavel:** [recipe/persisting-sessions.ts](recipe/persisting-sessions.ts)
>
> ```bash
> cd recipe && npm install
> npx tsx persisting-sessions.ts
> # or: npm run persisting-sessions
> ```

### Criando uma sessao com ID personalizado

```typescript
import { CopilotClient } from "@github/copilot-sdk";

const client = new CopilotClient();
await client.start();

// Create session with a memorable ID
const session = await client.createSession({
    sessionId: "user-123-conversation",
    model: "gpt-5",
});

await session.sendAndWait({ prompt: "Let's discuss TypeScript generics" });

// Session ID is preserved
console.log(session.sessionId); // "user-123-conversation"

// Destroy session but keep data on disk
await session.destroy();
await client.stop();
```

### Retomando uma sessao

```typescript
const client = new CopilotClient();
await client.start();

// Resume the previous session
const session = await client.resumeSession("user-123-conversation");

// Previous context is restored
await session.sendAndWait({ prompt: "What were we discussing?" });
// AI remembers the TypeScript generics discussion

await session.destroy();
await client.stop();
```

### Listando sessoes disponiveis

```typescript
const sessions = await client.listSessions();
console.log(sessions);
// [
//   { sessionId: "user-123-conversation", ... },
//   { sessionId: "user-456-conversation", ... },
// ]
```

### Excluindo uma sessao permanentemente

```typescript
// Remove session and all its data from disk
await client.deleteSession("user-123-conversation");
```

## Obtendo historico da sessao

Recupere todas as mensagens de uma sessao:

```typescript
const messages = await session.getMessages();
for (const msg of messages) {
    console.log(`[${msg.type}]`, msg.data);
}
```

## Melhores praticas

1. **Use IDs de sessao significativos**: Inclua ID do usuario ou contexto no ID da sessao
2. **Trate sessoes ausentes**: Verifique se a sessao existe antes de retomar
3. **Limpe sessoes antigas**: Periodicamente exclua sessoes que nao sao mais necessarias
