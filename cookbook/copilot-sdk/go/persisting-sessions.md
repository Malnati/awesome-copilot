# Persistencia e Retomada de Sessoes

Salve e restaure sessoes de conversa entre reinicios da aplicacao.

## Cenario de exemplo

Voce quer que os usuarios possam continuar uma conversa mesmo depois de fechar e reabrir seu aplicativo.

> **Exemplo executavel:** [recipe/persisting-sessions.go](recipe/persisting-sessions.go)
>
> ```bash
> cd recipe
> go run persisting-sessions.go
> ```

### Criando uma sessao com ID personalizado

```go
package main

import (
    "fmt"
    "github.com/github/copilot-sdk/go"
)

func main() {
    client := copilot.NewClient()
    client.Start()
    defer client.Stop()

    // Create session with a memorable ID
    session, _ := client.CreateSession(copilot.SessionConfig{
        SessionID: "user-123-conversation",
        Model:     "gpt-5",
    })

    session.Send(copilot.MessageOptions{Prompt: "Let's discuss TypeScript generics"})

    // Session ID is preserved
    fmt.Println(session.SessionID)

    // Destroy session but keep data on disk
    session.Destroy()
}
```

### Retomando uma sessao

```go
client := copilot.NewClient()
client.Start()
defer client.Stop()

// Resume the previous session
session, _ := client.ResumeSession("user-123-conversation")

// Previous context is restored
session.Send(copilot.MessageOptions{Prompt: "What were we discussing?"})

session.Destroy()
```

### Listando sessoes disponiveis

```go
sessions, _ := client.ListSessions()
for _, s := range sessions {
    fmt.Println("Session:", s.SessionID)
}
```

### Excluindo uma sessao permanentemente

```go
// Remove session and all its data from disk
client.DeleteSession("user-123-conversation")
```

### Obtendo historico da sessao

```go
messages, _ := session.GetMessages()
for _, msg := range messages {
    fmt.Printf("[%s] %v\n", msg.Type, msg.Data)
}
```

## Melhores praticas

1. **Use IDs de sessao significativos**: Inclua ID do usuario ou contexto no ID da sessao
2. **Trate sessoes ausentes**: Verifique se a sessao existe antes de retomar
3. **Limpe sessoes antigas**: Periodicamente exclua sessoes que nao sao mais necessarias
