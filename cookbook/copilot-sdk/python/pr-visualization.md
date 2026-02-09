# Gerando Graficos de Idade de PR

Crie uma ferramenta CLI interativa que visualiza a distribuicao de idade de pull requests em um repositorio GitHub usando os recursos integrados do Copilot.

> **Exemplo executavel:** [recipe/pr_visualization.py](recipe/pr_visualization.py)
>
> ```bash
> cd recipe && pip install -r requirements.txt
> # Auto-detect from current git repo
> python pr_visualization.py
>
> # Specify a repo explicitly
> python pr_visualization.py --repo github/copilot-sdk
> ```

## Cenario de exemplo

Voce quer entender ha quanto tempo PRs estao abertos em um repositorio. Esta ferramenta detecta o repo git atual ou aceita um repo como entrada, depois permite que o Copilot busque dados de PR via GitHub MCP Server e gere uma imagem de grafico.

## Pre-requisitos

```bash
pip install github-copilot-sdk
```

## Uso

```bash
# Auto-detect from current git repo
python pr_visualization.py

# Specify a repo explicitly
python pr_visualization.py --repo github/copilot-sdk
```

## Exemplo completo: pr_visualization.py

```python
#!/usr/bin/env python3

import subprocess
import sys
import os
from copilot import CopilotClient

# ============================================================================
# Git & GitHub Detection
# ============================================================================

def is_git_repo():
    try:
        subprocess.run(
            ["git", "rev-parse", "--git-dir"],
            check=True,
            capture_output=True
        )
        return True
    except (subprocess.CalledProcessError, FileNotFoundError):
        return False

def get_github_remote():
    try:
        result = subprocess.run(
            ["git", "remote", "get-url", "origin"],
            check=True,
            capture_output=True,
            text=True
        )
        remote_url = result.stdout.strip()

        # Handle SSH: git@github.com:owner/repo.git
        import re
        ssh_match = re.search(r"git@github\.com:(.+/.+?)(?:\.git)?$", remote_url)
        if ssh_match:
            return ssh_match.group(1)

        # Handle HTTPS: https://github.com/owner/repo.git
        https_match = re.search(r"https://github\.com/(.+/.+?)(?:\.git)?$", remote_url)
        if https_match:
            return https_match.group(1)

        return None
    except (subprocess.CalledProcessError, FileNotFoundError):
        return None

def parse_args():
    args = sys.argv[1:]
    if "--repo" in args:
        idx = args.index("--repo")
        if idx + 1 < len(args):
            return {"repo": args[idx + 1]}
    return {}

def prompt_for_repo():
    return input("Enter GitHub repo (owner/repo): ").strip()

# ============================================================================
# Main Application
# ============================================================================

def main():
    print("🔍 PR Age Chart Generator\n")

    # Determine the repository
    args = parse_args()
    repo = None

    if "repo" in args:
        repo = args["repo"]
        print(f"📦 Using specified repo: {repo}")
    elif is_git_repo():
        detected = get_github_remote()
        if detected:
            repo = detected
            print(f"📦 Detected GitHub repo: {repo}")
        else:
            print("⚠️  Git repo found but no GitHub remote detected.")
            repo = prompt_for_repo()
    else:
        print("📁 Not in a git repository.")
        repo = prompt_for_repo()

    if not repo or "/" not in repo:
        print("❌ Invalid repo format. Expected: owner/repo")
        sys.exit(1)

    owner, repo_name = repo.split("/", 1)

    # Create Copilot client - no custom tools needed!
    client = CopilotClient(log_level="error")
    client.start()

    session = client.create_session(
        model="gpt-5",
        system_message={
            "content": f"""
<context>
You are analyzing pull requests for the GitHub repository: {owner}/{repo_name}
The current working directory is: {os.getcwd()}
</context>

<instructions>
- Use the GitHub MCP Server tools to fetch PR data
- Use your file and code execution tools to generate charts
- Save any generated images to the current working directory
- Be concise in your responses
</instructions>
"""
        }
    )

    # Set up event handling
    def handle_event(event):
        if event["type"] == "assistant.message":
            print(f"\n🤖 {event['data']['content']}\n")
        elif event["type"] == "tool.execution_start":
            print(f"  ⚙️  {event['data']['toolName']}")

    session.on(handle_event)

    # Initial prompt - let Copilot figure out the details
    print("\n📊 Starting analysis...\n")

    session.send(prompt=f"""
      Fetch the open pull requests for {owner}/{repo_name} from the last week.
      Calculate the age of each PR in days.
      Then generate a bar chart image showing the distribution of PR ages
      (group them into sensible buckets like <1 day, 1-3 days, etc.).
      Save the chart as "pr-age-chart.png" in the current directory.
      Finally, summarize the PR health - average age, oldest PR, and how many might be considered stale.
    """)

    session.wait_for_idle()

    # Interactive loop
    print("\n💡 Ask follow-up questions or type \"exit\" to quit.\n")
    print("Examples:")
    print("  - \"Expand to the last month\"")
    print("  - \"Show me the 5 oldest PRs\"")
    print("  - \"Generate a pie chart instead\"")
    print("  - \"Group by author instead of age\"")
    print()

    while True:
        user_input = input("You: ").strip()

        if user_input.lower() in ["exit", "quit"]:
            print("👋 Goodbye!")
            break

        if user_input:
            session.send(prompt=user_input)
            session.wait_for_idle()

    session.destroy()
    client.stop()

if __name__ == "__main__":
    main()
```

## Como funciona

1. **Deteccao de repositorio**: Verifica a flag `--repo` → remote do git → solicita ao usuario
2. **Sem ferramentas personalizadas**: Depende apenas das capacidades integradas do Copilot CLI:
   - **GitHub MCP Server** - Busca dados de PR no GitHub
   - **Ferramentas de arquivo** - Salvam imagens de graficos geradas
   - **Execucao de codigo** - Gera graficos usando Python/matplotlib ou outros metodos
3. **Sessao interativa**: Apos a analise inicial, o usuario pode pedir ajustes

## Por que esta abordagem?

| Aspecto          | Ferramentas Personalizadas | Copilot Integrado               |
| --------------- | -------------------------- | ------------------------------- |
| Complexidade do codigo | Alta                  | **Minima**                      |
| Manutencao      | Voce mantem               | **Copilot mantem**              |
| Flexibilidade   | Logica fixa                | **IA decide a melhor abordagem**|
| Tipos de grafico| O que voce codou           | **Qualquer tipo que o Copilot gere** |
| Agrupamento de dados | Buckets fixos         | **Agrupamento inteligente**     |
