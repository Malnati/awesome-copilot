# Padroes de Tratamento de Erros

Trate erros com elegancia em suas aplicacoes do Copilot SDK.

> **Exemplo executavel:** [recipe/error_handling.py](recipe/error_handling.py)
>
> ```bash
> cd recipe && pip install -r requirements.txt
> python error_handling.py
> ```

## Cenario de exemplo

Voce precisa lidar com varias condicoes de erro como falhas de conexao, timeouts e respostas invalidas.

## Try-except basico

```python
from copilot import CopilotClient

client = CopilotClient()

try:
    client.start()
    session = client.create_session(model="gpt-5")

    response = None
    def handle_message(event):
        nonlocal response
        if event["type"] == "assistant.message":
            response = event["data"]["content"]

    session.on(handle_message)
    session.send(prompt="Hello!")
    session.wait_for_idle()

    if response:
        print(response)

    session.destroy()
except Exception as e:
    print(f"Error: {e}")
finally:
    client.stop()
```

## Tratando tipos de erro especificos

```python
import subprocess

try:
    client.start()
except FileNotFoundError:
    print("Copilot CLI not found. Please install it first.")
except ConnectionError:
    print("Could not connect to Copilot CLI server.")
except Exception as e:
    print(f"Unexpected error: {e}")
```

## Tratamento de timeout

```python
import signal
from contextlib import contextmanager

@contextmanager
def timeout(seconds):
    def timeout_handler(signum, frame):
        raise TimeoutError("Request timed out")

    old_handler = signal.signal(signal.SIGALRM, timeout_handler)
    signal.alarm(seconds)
    try:
        yield
    finally:
        signal.alarm(0)
        signal.signal(signal.SIGALRM, old_handler)

session = client.create_session(model="gpt-5")

try:
    session.send(prompt="Complex question...")

    # Wait with timeout (30 seconds)
    with timeout(30):
        session.wait_for_idle()

    print("Response received")
except TimeoutError:
    print("Request timed out")
```

## Abortando uma requisicao

```python
import threading

session = client.create_session(model="gpt-5")

# Start a request
session.send(prompt="Write a very long story...")

# Abort it after some condition
def abort_later():
    import time
    time.sleep(5)
    session.abort()
    print("Request aborted")

threading.Thread(target=abort_later).start()
```

## Encerramento elegante

```python
import signal
import sys

def signal_handler(sig, frame):
    print("\nShutting down...")
    errors = client.stop()
    if errors:
        print(f"Cleanup errors: {errors}")
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)
```

## Context manager para limpeza automatica

```python
from copilot import CopilotClient

with CopilotClient() as client:
    client.start()
    session = client.create_session(model="gpt-5")

    # ... do work ...

    # client.stop() is automatically called when exiting context
```

## Melhores praticas

1. **Sempre limpe**: Use try-finally ou context managers para garantir que `stop()` seja chamado
2. **Trate erros de conexao**: A CLI pode nao estar instalada ou em execucao
3. **Defina timeouts apropriados**: Requisicoes longas devem ter timeouts
4. **Registre erros**: Capture detalhes de erros para depuracao
