# Persistencia e Retomada de Sessoes

Salve e restaure sessoes de conversa entre reinicios da aplicacao.

## Cenario de exemplo

Voce quer que os usuarios possam continuar uma conversa mesmo depois de fechar e reabrir seu aplicativo.

> **Exemplo executavel:** [recipe/persisting-sessions.cs](recipe/persisting-sessions.cs)
>
> ```bash
> cd recipe
> dotnet run persisting-sessions.cs
> ```

### Criando uma sessao com ID personalizado

```csharp
using GitHub.Copilot.SDK;

await using var client = new CopilotClient();
await client.StartAsync();

// Create session with a memorable ID
var session = await client.CreateSessionAsync(new SessionConfig
{
    SessionId = "user-123-conversation",
    Model = "gpt-5"
});

await session.SendAsync(new MessageOptions { Prompt = "Let's discuss TypeScript generics" });

// Session ID is preserved
Console.WriteLine(session.SessionId); // "user-123-conversation"

// Destroy session but keep data on disk
await session.DisposeAsync();
await client.StopAsync();
```

### Retomando uma sessao

```csharp
await using var client = new CopilotClient();
await client.StartAsync();

// Resume the previous session
var session = await client.ResumeSessionAsync("user-123-conversation");

// Previous context is restored
await session.SendAsync(new MessageOptions { Prompt = "What were we discussing?" });

await session.DisposeAsync();
await client.StopAsync();
```

### Listando sessoes disponiveis

```csharp
var sessions = await client.ListSessionsAsync();
foreach (var s in sessions)
{
    Console.WriteLine($"Session: {s.SessionId}");
}
```

### Excluindo uma sessao permanentemente

```csharp
// Remove session and all its data from disk
await client.DeleteSessionAsync("user-123-conversation");
```

### Obtendo historico da sessao

Recupere todas as mensagens de uma sessao:

```csharp
var messages = await session.GetMessagesAsync();
foreach (var msg in messages)
{
    Console.WriteLine($"[{msg.Type}] {msg.Data.Content}");
}
```

## Melhores praticas

1. **Use IDs de sessao significativos**: Inclua ID do usuario ou contexto no ID da sessao
2. **Trate sessoes ausentes**: Verifique se a sessao existe antes de retomar
3. **Limpe sessoes antigas**: Periodicamente exclua sessoes que nao sao mais necessarias
