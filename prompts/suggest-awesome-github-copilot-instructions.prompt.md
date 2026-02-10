---
agent: 'agent'
description: 'Sugira arquivos de instrucoes GitHub Copilot relevantes do repositorio awesome-copilot com base no contexto atual do repositorio e historico do chat, evitando duplicatas com instrucoes existentes neste repositorio e identificando instrucoes desatualizadas que precisam de updates.'
tools: ['edit', 'search', 'runCommands', 'runTasks', 'think', 'changes', 'testFailure', 'openSimpleBrowser', 'web/fetch', 'githubRepo', 'todos', 'search']
---
# Sugerir Instrucoes do Awesome GitHub Copilot

Analise o contexto atual do repositorio e sugira arquivos de copilot-instructions relevantes do [repositorio awesome-copilot](https://github.com/Malnati/awesome-copilot/blob/main/docs/README.instructions.md) que ainda nao estao disponiveis neste repositorio.

## Processo

1. **Fetch Available Instructions**: Extraia lista de instrucoes e descricoes de [awesome-copilot README.instructions.md](https://github.com/Malnati/awesome-copilot/blob/main/docs/README.instructions.md). Deve usar a tool `#fetch`.
2. **Scan Local Instructions**: Descubra arquivos de instrucoes existentes na pasta `.github/instructions/`
3. **Extract Descriptions**: Leia front matter de arquivos locais para obter descricoes e padroes `applyTo`
4. **Fetch Remote Versions**: Para cada instrucao local, busque a versao correspondente no repositorio awesome-copilot usando URLs raw do GitHub (ex: `https://raw.githubusercontent.com/github/awesome-copilot/main/instructions/<filename>`)
5. **Compare Versions**: Compare o conteudo local com versoes remotas para identificar:
   - Instrucoes up-to-date (match exato)
   - Instrucoes desatualizadas (conteudo diferente)
   - Diferencas chave em instrucoes desatualizadas (descricao, applyTo, conteudo)
6. **Analyze Context**: Revise historico do chat, arquivos do repositorio e necessidades atuais do projeto
7. **Compare Existing**: Verifique instrucoes ja disponiveis neste repositorio
8. **Match Relevance**: Compare instrucoes disponiveis com padroes e requisitos identificados
9. **Present Options**: Exiba instrucoes relevantes com descricoes, rationale e status de disponibilidade incluindo instrucoes desatualizadas
10. **Validate**: Garanta que instrucoes sugeridas agregam valor nao coberto por instrucoes existentes
11. **Output**: Forneca tabela estruturada com sugestoes, descricoes e links para instrucoes do awesome-copilot e instrucoes locais similares
   **AGUARDE** solicitacao do usuario para prosseguir com instalacao ou updates de instrucoes especificas. NAO INSTALE OU ATUALIZE SEM SER DIRECIONADO.
12. **Download/Update Assets**: Para instrucoes solicitadas, automaticamente:
    - Baixe novas instrucoes para `.github/instructions/`
    - Atualize instrucoes desatualizadas substituindo pela versao mais recente do awesome-copilot
    - NAO ajuste conteudo dos arquivos
    - Use a tool `#fetch` para baixar assets, mas pode usar `curl` via `#runInTerminal` para garantir todo o conteudo
    - Use a tool `#todos` para acompanhar progresso

## Criterios de Analise de Contexto

🔍 **Repository Patterns**:
- Linguagens usadas (.cs, .js, .py, .ts, etc.)
- Indicadores de frameworks (ASP.NET, React, Azure, Next.js, etc.)
- Tipos de projeto (web apps, APIs, libraries, tools)
- Requisitos de workflow de desenvolvimento (testing, CI/CD, deployment)

🗨️ **Chat History Context**:
- Discussoes recentes e pontos de dor
- Perguntas especificas de tecnologia
- Discussao de padroes de codigo
- Requisitos de workflow de desenvolvimento

## Formato de Saida

Exiba resultados de analise em tabela estruturada comparando instrucoes do awesome-copilot com instrucoes existentes no repositorio:

| Awesome-Copilot Instruction | Description | Already Installed | Similar Local Instruction | Suggestion Rationale |
|------------------------------|-------------|-------------------|---------------------------|---------------------|
| [blazor.instructions.md](https://github.com/Malnati/awesome-copilot/blob/main/instructions/blazor.instructions.md) | Blazor development guidelines | ✅ Sim | blazor.instructions.md | Ja coberto por instrucoes existentes de Blazor |
| [reactjs.instructions.md](https://github.com/Malnati/awesome-copilot/blob/main/instructions/reactjs.instructions.md) | ReactJS development standards | ❌ Nao | None | Melhoraria desenvolvimento React com padroes estabelecidos |
| [java.instructions.md](https://github.com/Malnati/awesome-copilot/blob/main/instructions/java.instructions.md) | Java development best practices | ⚠️ Desatualizado | java.instructions.md | Padrao applyTo difere: remoto usa `'**/*.java'` vs local `'*.java'` - Update recomendado |

## Processo de Descoberta de Instrucoes Locais

1. Liste todos os arquivos `*.instructions.md` no diretorio `instructions/`
2. Para cada arquivo encontrado, leia o front matter para extrair `description` e `applyTo`
3. Monte inventario abrangente de instrucoes existentes com seus patterns de arquivo aplicaveis
4. Use este inventario para evitar sugerir duplicatas

## Processo de Comparacao de Versoes

1. Para cada arquivo de instrucao local, construa a URL raw do GitHub para buscar a versao remota:
   - Padrao: `https://raw.githubusercontent.com/github/awesome-copilot/main/instructions/<filename>`
2. Busque a versao remota usando a tool `#fetch`
3. Compare o conteudo completo do arquivo (incluindo front matter e corpo)
4. Identifique diferencas especificas:
   - **Mudancas de front matter** (description, applyTo)
   - **Atualizacoes de conteudo** (guidelines, exemplos, boas praticas)
5. Documente diferencas-chave para instrucoes desatualizadas
6. Calcule similaridade para determinar se update e necessario

## Requisitos de Estrutura de Arquivo

Com base na documentacao do GitHub, arquivos de copilot-instructions devem ser:
- **Repository-wide instructions**: `.github/copilot-instructions.md` (aplica ao repositorio todo)
- **Path-specific instructions**: `.github/instructions/NAME.instructions.md` (aplica a file patterns especificos via front matter `applyTo`)
- **Community instructions**: `instructions/NAME.instructions.md` (para compartilhamento e distribuicao)

## Estrutura de Front Matter

Arquivos de instrucoes no awesome-copilot usam este front matter:
```markdown
---
description: 'Brief description of what this instruction provides'
applyTo: '**/*.js,**/*.ts' # Optional: glob patterns for file matching
---
```

## Requisitos

- Use a tool `githubRepo` para obter conteudo da pasta instructions do awesome-copilot
- Scanne o file system local para instrucoes existentes em `.github/instructions/`
- Leia front matter YAML das instrucoes locais para extrair descricoes e `applyTo`
- Compare instrucoes locais com versoes remotas para detectar desatualizadas
- Compare com instrucoes existentes no repositorio para evitar duplicatas
- Foque em gaps na cobertura da biblioteca de instrucoes atual
- Valide que instrucoes sugeridas alinham com proposito e padroes do repositorio
- Forneca rationale claro para cada sugestao
- Inclua links para instrucoes do awesome-copilot e instrucoes locais similares
- Identifique claramente instrucoes desatualizadas com diferencas especificas
- Considere compatibilidade com stack e necessidades do projeto
- Nao forneca informacao ou contexto adicional alem da tabela e analise

## Referencia de Icones

- ✅ Ja instalado e atualizado
- ⚠️ Instalado mas desatualizado (update disponivel)
- ❌ Nao instalado no repo

## Tratamento de Updates

Quando instrucoes desatualizadas forem identificadas:
1. Inclua-as na tabela de saida com status ⚠️
2. Documente diferencas especificas na coluna "Suggestion Rationale"
3. Forneca recomendacao de update com mudancas-chave
