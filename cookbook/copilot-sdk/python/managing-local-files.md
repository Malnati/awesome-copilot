# Agrupando Arquivos por Metadados

Use o Copilot para organizar arquivos de forma inteligente em uma pasta com base em seus metadados.

> **Exemplo executavel:** [recipe/managing_local_files.py](recipe/managing_local_files.py)
>
> ```bash
> cd recipe && pip install -r requirements.txt
> python managing_local_files.py
> ```

## Cenario de exemplo

Voce tem uma pasta com muitos arquivos e quer organiza-los em subpastas com base em metadados como tipo de arquivo, data de criacao, tamanho ou outros atributos. O Copilot pode analisar os arquivos e sugerir ou executar uma estrategia de agrupamento.

## Codigo de exemplo

```python
from copilot import CopilotClient
import os

# Create and start client
client = CopilotClient()
client.start()

# Create session
session = client.create_session(model="gpt-5")

# Event handler
def handle_event(event):
    if event["type"] == "assistant.message":
        print(f"\nCopilot: {event['data']['content']}")
    elif event["type"] == "tool.execution_start":
        print(f"  → Running: {event['data']['toolName']}")
    elif event["type"] == "tool.execution_complete":
        print(f"  ✓ Completed: {event['data']['toolCallId']}")

session.on(handle_event)

# Ask Copilot to organize files
target_folder = os.path.expanduser("~/Downloads")

session.send(prompt=f"""
Analyze the files in "{target_folder}" and organize them into subfolders.

1. First, list all files and their metadata
2. Preview grouping by file extension
3. Create appropriate subfolders (e.g., "images", "documents", "videos")
4. Move each file to its appropriate subfolder

Please confirm before moving any files.
""")

session.wait_for_idle()

client.stop()
```

## Estrategias de agrupamento

### Por extensao de arquivo

```python
# Groups files like:
# images/   -> .jpg, .png, .gif
# documents/ -> .pdf, .docx, .txt
# videos/   -> .mp4, .avi, .mov
```

### Por data de criacao

```python
# Groups files like:
# 2024-01/ -> files created in January 2024
# 2024-02/ -> files created in February 2024
```

### Por tamanho de arquivo

```python
# Groups files like:
# tiny-under-1kb/
# small-under-1mb/
# medium-under-100mb/
# large-over-100mb/
```

## Modo dry-run

Por seguranca, voce pode pedir ao Copilot para apenas prever as alteracoes:

```python
session.send(prompt=f"""
Analyze files in "{target_folder}" and show me how you would organize them
by file type. DO NOT move any files - just show me the plan.
""")
```

## Agrupamento personalizado com analise de IA

Deixe o Copilot determinar o melhor agrupamento com base no conteudo dos arquivos:

```python
session.send(prompt=f"""
Look at the files in "{target_folder}" and suggest a logical organization.
Consider:
- File names and what they might contain
- File types and their typical uses
- Date patterns that might indicate projects or events

Propose folder names that are descriptive and useful.
""")
```

## Consideracoes de seguranca

1. **Confirme antes de mover**: Peça ao Copilot para confirmar antes de executar movimentacoes
2. **Trate duplicatas**: Considere o que acontece se existir um arquivo com o mesmo nome
3. **Preserve os originais**: Considere copiar em vez de mover para arquivos importantes
