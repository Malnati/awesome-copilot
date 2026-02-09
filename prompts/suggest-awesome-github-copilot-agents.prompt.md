---
agent: "agent"
description: "Sugira arquivos de GitHub Copilot Custom Agents relevantes do repositorio awesome-copilot com base no contexto atual do repositorio e historico do chat, evitando duplicatas com agentes existentes neste repositorio e identificando agentes desatualizados que precisam de updates."
tools: ["edit", "search", "runCommands", "runTasks", "changes", "testFailure", "openSimpleBrowser", "fetch", "githubRepo", "todos"]
---

# Sugerir Awesome GitHub Copilot Custom Agents

Analise o contexto atual do repositorio e sugira arquivos de Custom Agents relevantes do [repositorio awesome-copilot](https://github.com/github/awesome-copilot/blob/main/docs/README.agents.md) que ainda nao estao disponiveis neste repositorio. Os arquivos de Custom Agent estao na pasta [agents](https://github.com/github/awesome-copilot/tree/main/agents) do repositorio awesome-copilot.

## Processo

1. **Fetch Available Custom Agents**: Extraia a lista de Custom Agents e descricoes de [awesome-copilot README.agents.md](https://github.com/github/awesome-copilot/blob/main/docs/README.agents.md). Deve usar a tool `fetch`.
2. **Scan Local Custom Agents**: Descubra arquivos de custom agent existentes na pasta `.github/agents/`
3. **Extract Descriptions**: Leia front matter dos arquivos locais para obter descricoes
4. **Fetch Remote Versions**: Para cada agente local, busque a versao correspondente no repositorio awesome-copilot usando URLs raw do GitHub (ex: `https://raw.githubusercontent.com/github/awesome-copilot/main/agents/<filename>`)
5. **Compare Versions**: Compare o conteudo local com o remoto para identificar:
   - Agentes up-to-date (match exato)
   - Agentes desatualizados (conteudo diferente)
   - Diferencas chave nos agentes desatualizados (tools, descricao, conteudo)
6. **Analyze Context**: Revise historico do chat, arquivos do repositorio e necessidades atuais do projeto
7. **Match Relevance**: Compare agentes disponiveis com padroes e requisitos identificados
8. **Present Options**: Exiba agentes relevantes com descricoes, rationale e status de disponibilidade incluindo agentes desatualizados
9. **Validate**: Garanta que os agentes sugeridos agregam valor nao coberto por agentes existentes
10. **Output**: Forneca uma tabela estruturada com sugestoes, descricoes e links para agentes do awesome-copilot e agentes locais similares
    **AGUARDE** a solicitacao do usuario para prosseguir com instalacao ou updates de agentes especificos. NAO INSTALE OU ATUALIZE SEM SER DIRECIONADO.
11. **Download/Update Assets**: Para agentes solicitados, automaticamente:
    - Baixe novos agentes para `.github/agents/`
    - Atualize agentes desatualizados substituindo pela versao mais recente do awesome-copilot
    - NAO ajuste conteudo dos arquivos
    - Use a tool `#fetch` para baixar assets, mas pode usar `curl` via `#runInTerminal` para garantir todo o conteudo
    - Use a tool `#todos` para acompanhar progresso

## Criterios de Analise de Contexto

🔍 **Repository Patterns**:

- Linguagens usadas (.cs, .js, .py, etc.)
- Indicadores de frameworks (ASP.NET, React, Azure, etc.)
- Tipos de projeto (web apps, APIs, libraries, tools)
- Necessidades de documentacao (README, specs, ADRs)

🗨️ **Chat History Context**:

- Discussões recentes e pontos de dor
- Requisitos de feature ou implementacao
- Padroes de code review
- Requisitos de workflow de desenvolvimento

## Formato de Saida

Exiba resultados da analise em tabela estruturada comparando custom agents do awesome-copilot com os existentes no repositorio:

| Awesome-Copilot Custom Agent                                                                                                                            | Description                                                                                                                                                                | Already Installed | Similar Local Custom Agent         | Suggestion Rationale                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | ---------------------------------- | ------------------------------------------------------------- |
| [amplitude-experiment-implementation.agent.md](https://github.com/github/awesome-copilot/blob/main/agents/amplitude-experiment-implementation.agent.md) | This custom agent uses Amplitude's MCP tools to deploy new experiments inside of Amplitude, enabling seamless variant testing capabilities and rollout of product features | ❌ No             | None                               | Would enhance experimentation capabilities within the product |
| [launchdarkly-flag-cleanup.agent.md](https://github.com/github/awesome-copilot/blob/main/agents/launchdarkly-flag-cleanup.agent.md)                     | Feature flag cleanup agent for LaunchDarkly                                                                                                                                | ✅ Yes            | launchdarkly-flag-cleanup.agent.md | Already covered by existing LaunchDarkly custom agents        |
| [principal-software-engineer.agent.md](https://github.com/github/awesome-copilot/blob/main/agents/principal-software-engineer.agent.md)                 | Provide principal-level software engineering guidance with focus on engineering excellence, technical leadership, and pragmatic implementation.                            | ⚠️ Outdated       | principal-software-engineer.agent.md | Tools configuration differs: remote uses `'web/fetch'` vs local `'fetch'` - Update recommended |

## Local Agent Discovery Process

1. Liste todos os arquivos `*.agent.md` na pasta `.github/agents/`
2. Para cada arquivo encontrado, leia o front matter para extrair `description`
3. Monte inventario abrangente de agentes existentes
4. Use este inventario para evitar sugerir duplicatas

## Version Comparison Process

1. Para cada arquivo de agente local, construa a URL raw do GitHub para buscar a versao remota:
   - Padrao: `https://raw.githubusercontent.com/github/awesome-copilot/main/agents/<filename>`
2. Busque a versao remota usando a tool `fetch`
3. Compare o conteudo completo do arquivo (incluindo front matter, tools array e corpo)
4. Identifique diferencas especificas:
   - **Mudancas de front matter** (description, tools)
   - **Modificacoes no array de tools** (adicoes, remocoes, renomeacoes)
   - **Atualizacoes de conteudo** (instrucoes, exemplos, guidelines)
5. Documente diferencas-chave para agentes desatualizados
6. Calcule similaridade para determinar se update e necessario

## Requirements

- Use a tool `githubRepo` para obter conteudo da pasta agents do awesome-copilot
- Scanne o file system local para agentes existentes em `.github/agents/`
- Leia front matter YAML dos agentes locais para extrair descricoes
- Compare agentes locais com versoes remotas para detectar desatualizados
- Compare com agentes existentes no repositorio para evitar duplicatas
- Foque em gaps na cobertura da biblioteca de agentes atual
- Valide que agentes sugeridos alinham com o proposito e padroes do repositorio
- Forneca rationale claro para cada sugestao
- Inclua links para agentes do awesome-copilot e agentes locais similares
- Identifique claramente agentes desatualizados com diferencas especificas
- Nao forneca informacao ou contexto adicional alem da tabela e analise

## Icons Reference

- ✅ Ja instalado e atualizado
- ⚠️ Instalado mas desatualizado (update disponivel)
- ❌ Nao instalado no repo

## Update Handling

Quando agentes desatualizados forem identificados:
1. Inclua-os na tabela de saida com status ⚠️
2. Documente diferencas especificas na coluna "Suggestion Rationale"
3. Forneca recomendacao de update com mudancas-chave
4. Quando o usuario pedir update, substitua o arquivo local inteiro pela versao remota
5. Preserve a localizacao do arquivo em `.github/agents/`
