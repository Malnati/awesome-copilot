# Exemplos de Receita Executaveis

Esta pasta contem exemplos Python independentes e executaveis para cada receita do cookbook. Cada arquivo pode ser executado diretamente como script Python.

## Pre-requisitos

- Python 3.8 ou superior
- Instale as dependencias (isso instala o SDK do PyPI):

```bash
pip install -r requirements.txt
```

## Executando Exemplos

Cada arquivo `.py` e um programa completo e executavel com permissoes de execucao:

```bash
python <filename>.py
# ou em sistemas tipo Unix:
./<filename>.py
```

### Receitas Disponiveis

| Receita               | Comando                          | Descricao                                  |
| --------------------- | -------------------------------- | ------------------------------------------ |
| Error Handling        | `python error_handling.py`       | Demonstra padroes de tratamento de erros   |
| Multiple Sessions     | `python multiple_sessions.py`    | Gerencia multiplas conversas independentes |
| Managing Local Files  | `python managing_local_files.py` | Organiza arquivos com agrupamento por IA   |
| PR Visualization      | `python pr_visualization.py`     | Gera graficos de idade de PR               |
| Persisting Sessions   | `python persisting_sessions.py`  | Salva e retoma sessoes entre reinicios     |

### Exemplos com Argumentos

**PR Visualization com repo especifico:**

```bash
python pr_visualization.py --repo github/copilot-sdk
```

**Managing Local Files (edite o arquivo para mudar a pasta alvo):**

```bash
# Edit the target_folder variable in managing_local_files.py first
python managing_local_files.py
```

## Desenvolvimento do SDK Local

O `requirements.txt` instala o pacote Copilot SDK do PyPI. Isso significa:

- Voce recebe a versao estavel mais recente do SDK
- Nao ha necessidade de compilar a partir do codigo-fonte
- Perfeito para usar o SDK nos seus projetos

Se voce quiser usar uma versao local de desenvolvimento, edite o requirements.txt para usar `-e ../..` no modo editavel.

## Melhores Praticas em Python

Estes exemplos seguem convencoes de Python:

- Nomenclatura PEP 8 (snake_case para funcoes e variaveis)
- Linha shebang para execucao direta
- Tratamento adequado de excecoes
- Type hints quando apropriado
- Uso da biblioteca padrao

## Virtual Environment (Recomendado)

Para desenvolvimento isolado:

```bash
# Create virtual environment
python -m venv venv

# Activate it
# Windows:
venv\Scripts\activate
# Unix/macOS:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## Recursos de Aprendizado

- [Python Documentation](https://docs.python.org/3/)
- [PEP 8 Style Guide](https://pep8.org/)
- [GitHub Copilot SDK for Python](https://github.com/github/copilot-sdk/blob/main/python/README.md)
- [Cookbook Pai](../README.md)
