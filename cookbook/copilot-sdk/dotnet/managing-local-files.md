# Agrupando Arquivos por Metadados

Use o Copilot para organizar arquivos de forma inteligente em uma pasta com base em seus metadados.

> **Exemplo executavel:** [recipe/managing-local-files.cs](recipe/managing-local-files.cs)
>
> ```bash
> dotnet run recipe/managing-local-files.cs
> ```

## Cenario de exemplo

Voce tem uma pasta com muitos arquivos e quer organiza-los em subpastas com base em metadados como tipo de arquivo, data de criacao, tamanho ou outros atributos. O Copilot pode analisar os arquivos e sugerir ou executar uma estrategia de agrupamento.

## Codigo de exemplo

```csharp
using GitHub.Copilot.SDK;

// Create and start client
await using var client = new CopilotClient();
await client.StartAsync();

// Define tools for file operations
var session = await client.CreateSessionAsync(new SessionConfig
{
    Model = "gpt-5"
});

// Wait for completion
var done = new TaskCompletionSource();

session.On(evt =>
{
    switch (evt)
    {
        case AssistantMessageEvent msg:
            Console.WriteLine($"\nCopilot: {msg.Data.Content}");
            break;
        case ToolExecutionStartEvent toolStart:
            Console.WriteLine($"  → Running: {toolStart.Data.ToolName} ({toolStart.Data.ToolCallId})");
            break;
        case ToolExecutionCompleteEvent toolEnd:
            Console.WriteLine($"  ✓ Completed: {toolEnd.Data.ToolCallId}");
            break;
        case SessionIdleEvent:
            done.SetResult();
            break;
    }
});

// Ask Copilot to organize files
var targetFolder = @"C:\Users\Me\Downloads";

await session.SendAsync(new MessageOptions
{
    Prompt = $"""
        Analyze the files in "{targetFolder}" and organize them into subfolders.

        1. First, list all files and their metadata
        2. Preview grouping by file extension
        3. Create appropriate subfolders (e.g., "images", "documents", "videos")
        4. Move each file to its appropriate subfolder

        Please confirm before moving any files.
        """
});

await done.Task;
```

## Estrategias de agrupamento

### Por extensao de arquivo

```csharp
// Groups files like:
// images/   -> .jpg, .png, .gif
// documents/ -> .pdf, .docx, .txt
// videos/   -> .mp4, .avi, .mov
```

### Por data de criacao

```csharp
// Groups files like:
// 2024-01/ -> files created in January 2024
// 2024-02/ -> files created in February 2024
```

### Por tamanho de arquivo

```csharp
// Groups files like:
// tiny-under-1kb/
// small-under-1mb/
// medium-under-100mb/
// large-over-100mb/
```

## Modo dry-run

Por seguranca, voce pode pedir ao Copilot para apenas prever as alteracoes:

```csharp
await session.SendAsync(new MessageOptions
{
    Prompt = $"""
        Analyze files in "{targetFolder}" and show me how you would organize them
        by file type. DO NOT move any files - just show me the plan.
        """
});
```

## Agrupamento personalizado com analise de IA

Deixe o Copilot determinar o melhor agrupamento com base no conteudo dos arquivos:

```csharp
await session.SendAsync(new MessageOptions
{
    Prompt = $"""
        Look at the files in "{targetFolder}" and suggest a logical organization.
        Consider:
        - File names and what they might contain
        - File types and their typical uses
        - Date patterns that might indicate projects or events

        Propose folder names that are descriptive and useful.
        """
});
```

## Consideracoes de seguranca

1. **Confirme antes de mover**: Peça ao Copilot para confirmar antes de executar movimentacoes
1. **Trate duplicatas**: Considere o que acontece se existir um arquivo com o mesmo nome
1. **Preserve os originais**: Considere copiar em vez de mover para arquivos importantes
