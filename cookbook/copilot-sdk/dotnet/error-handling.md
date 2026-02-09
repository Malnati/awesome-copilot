# Padroes de Tratamento de Erros

Trate erros com elegancia em suas aplicacoes do Copilot SDK.

> **Exemplo executavel:** [recipe/error-handling.cs](recipe/error-handling.cs)
>
> ```bash
> dotnet run recipe/error-handling.cs
> ```

## Cenario de exemplo

Voce precisa lidar com varias condicoes de erro como falhas de conexao, timeouts e respostas invalidas.

## Try-catch basico

```csharp
using GitHub.Copilot.SDK;

var client = new CopilotClient();

try
{
    await client.StartAsync();
    var session = await client.CreateSessionAsync(new SessionConfig
    {
        Model = "gpt-5"
    });

    var done = new TaskCompletionSource<string>();
    session.On(evt =>
    {
        if (evt is AssistantMessageEvent msg)
        {
            done.SetResult(msg.Data.Content);
        }
    });

    await session.SendAsync(new MessageOptions { Prompt = "Hello!" });
    var response = await done.Task;
    Console.WriteLine(response);

    await session.DisposeAsync();
}
catch (Exception ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
finally
{
    await client.StopAsync();
}
```

## Tratando tipos de erro especificos

```csharp
try
{
    await client.StartAsync();
}
catch (FileNotFoundException)
{
    Console.WriteLine("Copilot CLI not found. Please install it first.");
}
catch (HttpRequestException ex) when (ex.Message.Contains("connection"))
{
    Console.WriteLine("Could not connect to Copilot CLI server.");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

## Tratamento de timeout

```csharp
var session = await client.CreateSessionAsync(new SessionConfig { Model = "gpt-5" });

try
{
    var done = new TaskCompletionSource<string>();
    session.On(evt =>
    {
        if (evt is AssistantMessageEvent msg)
        {
            done.SetResult(msg.Data.Content);
        }
    });

    await session.SendAsync(new MessageOptions { Prompt = "Complex question..." });

    // Wait with timeout (30 seconds)
    using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));
    var response = await done.Task.WaitAsync(cts.Token);

    Console.WriteLine(response);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Request timed out");
}
```

## Abortando uma solicitacao

```csharp
var session = await client.CreateSessionAsync(new SessionConfig { Model = "gpt-5" });

// Start a request
await session.SendAsync(new MessageOptions { Prompt = "Write a very long story..." });

// Abort it after some condition
await Task.Delay(5000);
await session.AbortAsync();
Console.WriteLine("Request aborted");
```

## Encerramento elegante

```csharp
Console.CancelKeyPress += async (sender, e) =>
{
    e.Cancel = true;
    Console.WriteLine("Shutting down...");

    var errors = await client.StopAsync();
    if (errors.Count > 0)
    {
        Console.WriteLine($"Cleanup errors: {string.Join(", ", errors)}");
    }

    Environment.Exit(0);
};
```

## Usando await using para descarte automatico

```csharp
await using var client = new CopilotClient();
await client.StartAsync();

var session = await client.CreateSessionAsync(new SessionConfig { Model = "gpt-5" });

// ... do work ...

// client.StopAsync() is automatically called when exiting scope
```

## Melhores praticas

1. **Sempre limpe**: Use try-finally ou `await using` para garantir que `StopAsync()` seja chamado
2. **Trate erros de conexao**: A CLI pode nao estar instalada ou em execucao
3. **Defina timeouts apropriados**: Use `CancellationToken` para requisicoes de longa duracao
4. **Registre erros**: Capture detalhes de erros para depuracao
