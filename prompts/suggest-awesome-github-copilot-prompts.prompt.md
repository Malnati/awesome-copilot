---
agent: 'agent'
description: 'Sugira arquivos de prompt GitHub Copilot relevantes do repositorio awesome-copilot com base no contexto atual do repositorio e historico do chat, evitando duplicatas com prompts existentes neste repositorio e identificando prompts desatualizados que precisam de updates.'
tools: ['edit', 'search', 'runCommands', 'runTasks', 'think', 'changes', 'testFailure', 'openSimpleBrowser', 'web/fetch', 'githubRepo', 'todos', 'search']
---
# Sugerir Prompts do Awesome GitHub Copilot

Analise o contexto atual do repositorio e sugira arquivos de prompt relevantes do [repositorio awesome-copilot](https://github.com/github/awesome-copilot/blob/main/docs/README.prompts.md) que ainda nao estao disponiveis neste repositorio.

## Processo

1. **Fetch Available Prompts**: Extraia lista de prompts e descricoes de [awesome-copilot README.prompts.md](https://github.com/github/awesome-copilot/blob/main/docs/README.prompts.md). Deve usar a tool `#fetch`.
2. **Scan Local Prompts**: Descubra arquivos de prompt existentes na pasta `.github/prompts/`
3. **Extract Descriptions**: Leia front matter dos prompts locais para obter descricoes
4. **Fetch Remote Versions**: Para cada prompt local, busque a versao correspondente no repositorio awesome-copilot usando URLs raw do GitHub (ex: `https://raw.githubusercontent.com/github/awesome-copilot/main/prompts/<filename>`)
5. **Compare Versions**: Compare o conteudo local com versoes remotas para identificar:
   - Prompts up-to-date (match exato)
   - Prompts desatualizados (conteudo diferente)
   - Diferencas chave em prompts desatualizados (tools, descricao, conteudo)
6. **Analyze Context**: Revise historico do chat, arquivos do repositorio e necessidades atuais
7. **Compare Existing**: Verifique prompts ja disponiveis neste repositorio
8. **Match Relevance**: Compare prompts disponiveis com padroes e requisitos identificados
9. **Present Options**: Exiba prompts relevantes com descricoes, rationale e status de disponibilidade incluindo prompts desatualizados
10. **Validate**: Garanta que prompts sugeridos agregam valor nao coberto por prompts existentes
11. **Output**: Forneca tabela estruturada com sugestoes, descricoes e links para prompts do awesome-copilot e prompts locais similares
    **AGUARDE** solicitacao do usuario para prosseguir com instalacao ou updates de prompts especificos. NAO INSTALE OU ATUALIZE SEM SER DIRECIONADO.
12. **Download/Update Assets**: Para prompts solicitados, automaticamente:
    - Baixe novos prompts para `.github/prompts/`
    - Atualize prompts desatualizados substituindo pela versao mais recente do awesome-copilot
    - NAO ajuste o conteudo dos arquivos
    - Use a tool `#fetch` para baixar assets, mas pode usar `curl` via `#runInTerminal` para garantir todo o conteudo
    - Use a tool `#todos` para acompanhar progresso

## Criterios de Analise de Contexto

🔍 **Repository Patterns**:
- Linguagens usadas (.cs, .js, .py, etc.)
- Indicadores de frameworks (ASP.NET, React, Azure, etc.)
- Tipos de projeto (web apps, APIs, libraries, tools)
- Necessidades de documentacao (README, specs, ADRs)

🗨️ **Chat History Context**:
- Discussoes recentes e pontos de dor
- Requisitos de features ou implementacao
- Padroes de code review
- Requisitos de workflow de desenvolvimento

## Formato de Saida

Exiba resultados de analise em tabela estruturada comparando prompts do awesome-copilot com prompts existentes no repositorio:

| Awesome-Copilot Prompt | Description | Already Installed | Similar Local Prompt | Suggestion Rationale |
|-------------------------|-------------|-------------------|---------------------|---------------------|
| [code-review.prompt.md](https://github.com/github/awesome-copilot/blob/main/prompts/code-review.prompt.md) | Automated code review prompts | ❌ Nao | None | Melhoraria o workflow de desenvolvimento com processos padronizados de code review |
| [documentation.prompt.md](https://github.com/github/awesome-copilot/blob/main/prompts/documentation.prompt.md) | Generate project documentation | ✅ Sim | create_oo_component_documentation.prompt.md | Ja coberto por prompts existentes de documentacao |
| [debugging.prompt.md](https://github.com/github/awesome-copilot/blob/main/prompts/debugging.prompt.md) | Debug assistance prompts | ⚠️ Desatualizado | debugging.prompt.md | Configuracao de tools difere: remoto usa `'codebase'` vs local ausente - Update recomendado |

## Processo de Descoberta de Prompts Locais

1. Liste todos os arquivos `*.prompt.md` no diretorio `.github/prompts/`
2. Para cada arquivo encontrado, leia o front matter para extrair `description`
3. Monte um inventario abrangente de prompts existentes
4. Use esse inventario para evitar sugerir duplicatas

## Processo de Comparacao de Versoes

1. Para cada arquivo de prompt local, construa a URL raw do GitHub para buscar a versao remota:
   - Padrao: `https://raw.githubusercontent.com/github/awesome-copilot/main/prompts/<filename>`
2. Busque a versao remota usando a tool `#fetch`
3. Compare o conteudo completo do arquivo (incluindo front matter e corpo)
4. Identifique diferencas especificas:
   - **Mudancas de front matter** (description, tools, mode)
   - **Modificacoes no array de tools** (adicoes, remocoes, renomeacoes)
   - **Atualizacoes de conteudo** (instrucoes, exemplos, guidelines)
5. Documente diferencas-chave para prompts desatualizados
6. Calcule similaridade para determinar se update e necessario

## Requisitos

- Use a tool `githubRepo` para obter conteudo da pasta prompts do awesome-copilot
- Scanne o file system local para prompts existentes em `.github/prompts/`
- Leia front matter YAML dos prompts locais para extrair descricoes
- Compare prompts locais com versoes remotas para detectar prompts desatualizados
- Compare com prompts existentes no repositorio para evitar duplicatas
- Foque em gaps na cobertura da biblioteca de prompts atual
- Valide que prompts sugeridos alinham com o proposito e padroes do repositorio
- Forneca rationale claro para cada sugestao
- Inclua links para prompts do awesome-copilot e prompts locais similares
- Identifique claramente prompts desatualizados com diferencas especificas
- Nao forneca informacao ou contexto adicional alem da tabela e analise

## Referencia de Icones

- ✅ Ja instalado e atualizado
- ⚠️ Instalado mas desatualizado (update disponivel)
- ❌ Nao instalado no repo

## Tratamento de Updates

Quando prompts desatualizados forem identificados:
1. Inclua-os na tabela de saida com status ⚠️
2. Documente diferencas especificas na coluna "Suggestion Rationale"
3. Forneca recomendacao de update com mudancas-chave
4. Quando o usuario pedir update, substitua o arquivo local inteiro pela versao remota
5. Preserve a localizacao do arquivo no diretorio `.github/prompts/`
