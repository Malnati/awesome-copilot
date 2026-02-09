# Trabalhando com Multiplas Sessoes

Gerencie varias conversas independentes simultaneamente.

> **Exemplo executavel:** [recipe/multiple-sessions.cs](recipe/multiple-sessions.cs)
>
> ```bash
> dotnet run recipe/multiple-sessions.cs
> ```

## Cenario de exemplo

Voce precisa executar varias conversas em paralelo, cada uma com seu proprio contexto e historico.

## C#

```csharp
using GitHub.Copilot.SDK;

await using var client = new CopilotClient();
await client.StartAsync();

// Create multiple independent sessions
var session1 = await client.CreateSessionAsync(new SessionConfig { Model = "gpt-5" });
var session2 = await client.CreateSessionAsync(new SessionConfig { Model = "gpt-5" });
var session3 = await client.CreateSessionAsync(new SessionConfig { Model = "claude-sonnet-4.5" });

// Each session maintains its own conversation history
await session1.SendAsync(new MessageOptions { Prompt = "You are helping with a Python project" });
await session2.SendAsync(new MessageOptions { Prompt = "You are helping with a TypeScript project" });
await session3.SendAsync(new MessageOptions { Prompt = "You are helping with a Go project" });

// Follow-up messages stay in their respective contexts
await session1.SendAsync(new MessageOptions { Prompt = "How do I create a virtual environment?" });
await session2.SendAsync(new MessageOptions { Prompt = "How do I set up tsconfig?" });
await session3.SendAsync(new MessageOptions { Prompt = "How do I initialize a module?" });

// Clean up all sessions
await session1.DisposeAsync();
await session2.DisposeAsync();
await session3.DisposeAsync();
```

## IDs de sessao personalizados

Use IDs personalizados para facilitar o acompanhamento:

```csharp
var session = await client.CreateSessionAsync(new SessionConfig
{
    SessionId = "user-123-chat",
    Model = "gpt-5"
});

Console.WriteLine(session.SessionId); // "user-123-chat"
```

## Listando sessoes

```csharp
var sessions = await client.ListSessionsAsync();
foreach (var sessionInfo in sessions)
{
    Console.WriteLine($"Session: {sessionInfo.SessionId}");
}
```

## Excluindo sessoes

```csharp
// Delete a specific session
await client.DeleteSessionAsync("user-123-chat");
```

## Casos de uso

- **Aplicacoes multiusuario**: Uma sessao por usuario
- **Fluxos de trabalho multitarefa**: Sessoes separadas para tarefas diferentes
- **Testes A/B**: Compare respostas de modelos diferentes
