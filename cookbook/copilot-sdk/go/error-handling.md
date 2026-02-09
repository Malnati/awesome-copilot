# Padroes de Tratamento de Erros

Trate erros com elegancia em suas aplicacoes do Copilot SDK.

> **Exemplo executavel:** [recipe/error-handling.go](recipe/error-handling.go)
>
> ```bash
> go run recipe/error-handling.go
> ```

## Cenario de exemplo

Voce precisa lidar com varias condicoes de erro como falhas de conexao, timeouts e respostas invalidas.

## Tratamento de erro basico

```go
package main

import (
    "fmt"
    "log"
    "github.com/github/copilot-sdk/go"
)

func main() {
    client := copilot.NewClient()

    if err := client.Start(); err != nil {
        log.Fatalf("Failed to start client: %v", err)
    }
    defer func() {
        if err := client.Stop(); err != nil {
            log.Printf("Error stopping client: %v", err)
        }
    }()

    session, err := client.CreateSession(copilot.SessionConfig{
        Model: "gpt-5",
    })
    if err != nil {
        log.Fatalf("Failed to create session: %v", err)
    }
    defer session.Destroy()

    responseChan := make(chan string, 1)
    session.On(func(event copilot.Event) {
        if msg, ok := event.(copilot.AssistantMessageEvent); ok {
            responseChan <- msg.Data.Content
        }
    })

    if err := session.Send(copilot.MessageOptions{Prompt: "Hello!"}); err != nil {
        log.Printf("Failed to send message: %v", err)
    }

    response := <-responseChan
    fmt.Println(response)
}
```

## Tratando tipos de erro especificos

```go
import (
    "errors"
    "os/exec"
)

func startClient() error {
    client := copilot.NewClient()

    if err := client.Start(); err != nil {
        var execErr *exec.Error
        if errors.As(err, &execErr) {
            return fmt.Errorf("Copilot CLI not found. Please install it first: %w", err)
        }
        if errors.Is(err, context.DeadlineExceeded) {
            return fmt.Errorf("Could not connect to Copilot CLI server: %w", err)
        }
        return fmt.Errorf("Unexpected error: %w", err)
    }

    return nil
}
```

## Tratamento de timeout

```go
import (
    "context"
    "time"
)

func sendWithTimeout(session *copilot.Session) error {
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()

    responseChan := make(chan string, 1)
    errChan := make(chan error, 1)

    session.On(func(event copilot.Event) {
        if msg, ok := event.(copilot.AssistantMessageEvent); ok {
            responseChan <- msg.Data.Content
        }
    })

    if err := session.Send(copilot.MessageOptions{Prompt: "Complex question..."}); err != nil {
        return err
    }

    select {
    case response := <-responseChan:
        fmt.Println(response)
        return nil
    case err := <-errChan:
        return err
    case <-ctx.Done():
        return fmt.Errorf("request timed out")
    }
}
```

## Abortando uma requisicao

```go
func abortAfterDelay(session *copilot.Session) {
    // Start a request
    session.Send(copilot.MessageOptions{Prompt: "Write a very long story..."})

    // Abort it after some condition
    time.AfterFunc(5*time.Second, func() {
        if err := session.Abort(); err != nil {
            log.Printf("Failed to abort: %v", err)
        }
        fmt.Println("Request aborted")
    })
}
```

## Encerramento elegante

```go
import (
    "os"
    "os/signal"
    "syscall"
)

func main() {
    client := copilot.NewClient()

    // Set up signal handling
    sigChan := make(chan os.Signal, 1)
    signal.Notify(sigChan, os.Interrupt, syscall.SIGTERM)

    go func() {
        <-sigChan
        fmt.Println("\nShutting down...")

        if err := client.Stop(); err != nil {
            log.Printf("Cleanup errors: %v", err)
        }

        os.Exit(0)
    }()

    if err := client.Start(); err != nil {
        log.Fatal(err)
    }

    // ... do work ...
}
```

## Padrao de limpeza com defer

```go
func doWork() error {
    client := copilot.NewClient()

    if err := client.Start(); err != nil {
        return fmt.Errorf("failed to start: %w", err)
    }
    defer client.Stop()

    session, err := client.CreateSession(copilot.SessionConfig{Model: "gpt-5"})
    if err != nil {
        return fmt.Errorf("failed to create session: %w", err)
    }
    defer session.Destroy()

    // ... do work ...

    return nil
}
```

## Melhores praticas

1. **Sempre limpe**: Use defer para garantir que `Stop()` seja chamado
2. **Trate erros de conexao**: A CLI pode nao estar instalada ou em execucao
3. **Defina timeouts apropriados**: Use `context.WithTimeout` para requisicoes de longa duracao
4. **Registre erros**: Capture detalhes de erros para depuracao
5. **Encapsule erros**: Use `fmt.Errorf` com `%w` para preservar cadeias de erro
