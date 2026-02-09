# Agrupando Arquivos por Metadados

Use o Copilot para organizar arquivos de forma inteligente em uma pasta com base em seus metadados.

> **Exemplo executavel:** [recipe/managing-local-files.go](recipe/managing-local-files.go)
>
> ```bash
> go run recipe/managing-local-files.go
> ```

## Cenario de exemplo

Voce tem uma pasta com muitos arquivos e quer organiza-los em subpastas com base em metadados como tipo de arquivo, data de criacao, tamanho ou outros atributos. O Copilot pode analisar os arquivos e sugerir ou executar uma estrategia de agrupamento.

## Codigo de exemplo

```go
package main

import (
    "fmt"
    "log"
    "os"
    "path/filepath"
    "github.com/github/copilot-sdk/go"
)

func main() {
    // Create and start client
    client := copilot.NewClient()
    if err := client.Start(); err != nil {
        log.Fatal(err)
    }
    defer client.Stop()

    // Create session
    session, err := client.CreateSession(copilot.SessionConfig{
        Model: "gpt-5",
    })
    if err != nil {
        log.Fatal(err)
    }
    defer session.Destroy()

    // Event handler
    session.On(func(event copilot.Event) {
        switch e := event.(type) {
        case copilot.AssistantMessageEvent:
            fmt.Printf("\nCopilot: %s\n", e.Data.Content)
        case copilot.ToolExecutionStartEvent:
            fmt.Printf("  → Running: %s\n", e.Data.ToolName)
        case copilot.ToolExecutionCompleteEvent:
            fmt.Printf("  ✓ Completed: %s\n", e.Data.ToolName)
        }
    })

    // Ask Copilot to organize files
    homeDir, _ := os.UserHomeDir()
    targetFolder := filepath.Join(homeDir, "Downloads")

    prompt := fmt.Sprintf(`
Analyze the files in "%s" and organize them into subfolders.

1. First, list all files and their metadata
2. Preview grouping by file extension
3. Create appropriate subfolders (e.g., "images", "documents", "videos")
4. Move each file to its appropriate subfolder

Please confirm before moving any files.
`, targetFolder)

    if err := session.Send(copilot.MessageOptions{Prompt: prompt}); err != nil {
        log.Fatal(err)
    }

    session.WaitForIdle()
}
```

## Estrategias de agrupamento

### Por extensao de arquivo

```go
// Groups files like:
// images/   -> .jpg, .png, .gif
// documents/ -> .pdf, .docx, .txt
// videos/   -> .mp4, .avi, .mov
```

### Por data de criacao

```go
// Groups files like:
// 2024-01/ -> files created in January 2024
// 2024-02/ -> files created in February 2024
```

### Por tamanho de arquivo

```go
// Groups files like:
// tiny-under-1kb/
// small-under-1mb/
// medium-under-100mb/
// large-over-100mb/
```

## Modo dry-run

Por seguranca, voce pode pedir ao Copilot para apenas prever as alteracoes:

```go
prompt := fmt.Sprintf(`
Analyze files in "%s" and show me how you would organize them
by file type. DO NOT move any files - just show me the plan.
`, targetFolder)

session.Send(copilot.MessageOptions{Prompt: prompt})
```

## Agrupamento personalizado com analise de IA

Deixe o Copilot determinar o melhor agrupamento com base no conteudo dos arquivos:

```go
prompt := fmt.Sprintf(`
Look at the files in "%s" and suggest a logical organization.
Consider:
- File names and what they might contain
- File types and their typical uses
- Date patterns that might indicate projects or events

Propose folder names that are descriptive and useful.
`, targetFolder)

session.Send(copilot.MessageOptions{Prompt: prompt})
```

## Consideracoes de seguranca

1. **Confirme antes de mover**: Peça ao Copilot para confirmar antes de executar movimentacoes
2. **Trate duplicatas**: Considere o que acontece se existir um arquivo com o mesmo nome
3. **Preserve os originais**: Considere copiar em vez de mover para arquivos importantes
