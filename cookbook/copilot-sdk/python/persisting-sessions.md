# Persistencia e Retomada de Sessoes

Salve e restaure sessoes de conversa entre reinicios da aplicacao.

## Cenario de exemplo

Voce quer que os usuarios possam continuar uma conversa mesmo depois de fechar e reabrir seu aplicativo.

> **Exemplo executavel:** [recipe/persisting_sessions.py](recipe/persisting_sessions.py)
>
> ```bash
> cd recipe && pip install -r requirements.txt
> python persisting_sessions.py
> ```

### Criando uma sessao com ID personalizado

```python
from copilot import CopilotClient

client = CopilotClient()
client.start()

# Create session with a memorable ID
session = client.create_session(
    session_id="user-123-conversation",
    model="gpt-5",
)

session.send(prompt="Let's discuss TypeScript generics")

# Session ID is preserved
print(session.session_id)  # "user-123-conversation"

# Destroy session but keep data on disk
session.destroy()
client.stop()
```

### Retomando uma sessao

```python
client = CopilotClient()
client.start()

# Resume the previous session
session = client.resume_session("user-123-conversation")

# Previous context is restored
session.send(prompt="What were we discussing?")

session.destroy()
client.stop()
```

### Listando sessoes disponiveis

```python
sessions = client.list_sessions()
for s in sessions:
    print("Session:", s["sessionId"])
```

### Excluindo uma sessao permanentemente

```python
# Remove session and all its data from disk
client.delete_session("user-123-conversation")
```

### Obtendo historico da sessao

```python
messages = session.get_messages()
for msg in messages:
    print(f"[{msg['type']}] {msg['data']}")
```

## Melhores praticas

1. **Use IDs de sessao significativos**: Inclua ID do usuario ou contexto no ID da sessao
2. **Trate sessoes ausentes**: Verifique se a sessao existe antes de retomar
3. **Limpe sessoes antigas**: Periodicamente exclua sessoes que nao sao mais necessarias
