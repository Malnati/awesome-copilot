---
agent: 'agent'
description: 'Sugira collections relevantes do GitHub Copilot do repositorio awesome-copilot com base no contexto atual e historico do chat, oferecendo download e instalacao automatica de assets da collection e identificando assets desatualizados que precisam de updates.'
tools: ['edit', 'search', 'runCommands', 'runTasks', 'think', 'changes', 'testFailure', 'openSimpleBrowser', 'web/fetch', 'githubRepo', 'todos', 'search']
---
# Sugerir Collections do Awesome GitHub Copilot

Analise o contexto atual do repositorio e sugira collections relevantes do [repositorio awesome-copilot](https://github.com/github/awesome-copilot/blob/main/docs/README.collections.md) que melhorem o workflow de desenvolvimento deste repositorio.

## Processo

1. **Fetch Available Collections**: Extraia lista de collections e descricoes de [awesome-copilot README.collections.md](https://github.com/github/awesome-copilot/blob/main/docs/README.collections.md). Deve usar a tool `#fetch`.
2. **Scan Local Assets**: Descubra arquivos de prompt em `prompts/`, instrucoes em `instructions/`, e chat modes em `agents/`
3. **Extract Local Descriptions**: Leia front matter dos assets locais para entender capacidades existentes
4. **Fetch Remote Versions**: Para cada asset local que corresponda a item de collection, busque a versao correspondente no repositorio awesome-copilot usando URLs raw do GitHub (ex: `https://raw.githubusercontent.com/github/awesome-copilot/main/<type>/<filename>`)
5. **Compare Versions**: Compare conteudo local com versoes remotas para identificar:
   - Assets up-to-date (match exato)
   - Assets desatualizados (conteudo diferente)
   - Diferencas chave em assets desatualizados (tools, descricao, conteudo)
6. **Analyze Repository Context**: Revise historico do chat, arquivos do repositorio, linguagens, frameworks e necessidades atuais
7. **Match Collection Relevance**: Compare collections disponiveis com padroes e requisitos identificados
8. **Check Asset Overlap**: Para collections relevantes, analise itens individuais para evitar duplicatas com assets existentes
9. **Present Collection Options**: Exiba collections relevantes com descricoes, contagem de itens, contagem de assets desatualizados e rationale
10. **Provide Usage Guidance**: Explique como a collection instalada melhora o workflow
    **AGUARDE** a solicitacao do usuario para prosseguir com instalacao ou updates de collections especificas. NAO INSTALE OU ATUALIZE SEM SER DIRECIONADO.
11. **Download/Update Assets**: Para collections solicitadas, automaticamente:
    - Baixe novos assets para os diretorios apropriados
    - Atualize assets desatualizados substituindo pela versao mais recente do awesome-copilot
    - NAO ajuste conteudo dos arquivos
    - Use a tool `#fetch` para baixar assets, mas pode usar `curl` via `#runInTerminal` para garantir todo o conteudo

## Criterios de Analise de Contexto

🔍 **Repository Patterns**:
- Linguagens usadas (.cs, .js, .py, .ts, .bicep, .tf, etc.)
- Indicadores de frameworks (ASP.NET, React, Azure, Next.js, Angular, etc.)
- Tipos de projeto (web apps, APIs, libraries, tools, infraestrutura)
- Necessidades de documentacao (README, specs, ADRs, decisoes arquiteturais)
- Indicadores de workflow de desenvolvimento (CI/CD, testes, deploy)

🗨️ **Chat History Context**:
- Discucoes recentes e pontos de dor
- Requisitos de features ou implementacao
- Padroes de code review e preocupacoes de qualidade
- Requisitos e desafios de workflow de desenvolvimento
- Decisoes de stack e arquitetura

## Formato de Saida

Exiba resultados de analise em tabela estruturada mostrando collections relevantes e seu valor potencial:

### Recomendacoes de Collections

| Collection Name | Description | Items | Asset Overlap | Suggestion Rationale |
|-----------------|-------------|-------|---------------|---------------------|
| [Azure & Cloud Development](https://github.com/github/awesome-copilot/blob/main/collections/azure-cloud-development.md) | Ferramentas abrangentes de desenvolvimento Azure incluindo Infrastructure as Code, serverless functions, padroes de arquitetura e otimizacao de custos | 15 items | 3 similares | Melhoraria o workflow de desenvolvimento Azure com Bicep, Terraform e ferramentas de otimizacao de custos |
| [C# .NET Development](https://github.com/github/awesome-copilot/blob/main/collections/csharp-dotnet-development.md) | Prompts, instructions e chat modes essenciais para desenvolvimento C# e .NET incluindo testes, documentacao e boas praticas | 7 items | 2 similares | Ja coberto por assets .NET existentes, mas inclui padroes avancados de testes |
| [Testing & Test Automation](https://github.com/github/awesome-copilot/blob/main/collections/testing-automation.md) | Collection abrangente para escrita de testes, automacao de testes e test-driven development | 11 items | 1 similar | Pode melhorar significativamente praticas de teste com orientacao de TDD e ferramentas de automacao |

### Analise de Assets para Collections Recomendadas

Para cada collection sugerida, detalhar assets individuais:

**Azure & Cloud Development Collection Analysis:**
- ✅ **Assets Novos (12)**: prompts de otimizacao de custos Azure, modo de planejamento Bicep, modulos AVM, modo especialista de Logic Apps
- ⚠️ **Assets Similares (3)**: pipelines Azure DevOps (similares a CI/CD existente), Terraform (overlap basico), containerizacao (Docker basics coberto)
- 🔄 **Assets Desatualizados (2)**: azure-iac-generator.agent.md (tools atualizadas), bicep-implement.agent.md (descricao alterada)
- 🎯 **Alto Valor**: ferramentas de otimizacao de custos, expertise de Infrastructure as Code, orientacao arquitetural Azure\n\n**Preview de Instalacao:**\n- Instalara em `prompts/`: 4 prompts especificos de Azure\n- Instalara em `instructions/`: 6 boas praticas de infraestrutura e DevOps\n- Instalara em `agents/`: 5 modos especialistas Azure

## Processo de Descoberta de Assets Locais

1. **Scan Asset Directories**:
   - Liste todos os arquivos `*.prompt.md` em `prompts/`
   - Liste todos os arquivos `*.instructions.md` em `instructions/`
   - Liste todos os arquivos `*.agent.md` em `agents/`

2. **Extract Asset Metadata**: Para cada arquivo encontrado, leia o front matter YAML para extrair:
   - `description` - Proposito e funcionalidade
   - `tools` - Tools e capacidades necessarias
   - `mode` - Modo de operacao (para prompts)
   - `model` - Requisitos de modelo (para chat modes)

3. **Build Asset Inventory**: Crie um mapa abrangente de capacidades existentes organizado por:
   - **Technology Focus**: Linguagens, frameworks, plataformas
   - **Workflow Type**: Desenvolvimento, testes, deploy, documentacao, planejamento
   - **Specialization Level**: Proposito geral vs. modos especialistas

4. **Identify Coverage Gaps**: Compare assets existentes com:
   - Requisitos da stack do repositorio
   - Necessidades de workflow indicadas pelo historico do chat
   - Boas praticas do setor para os tipos de projeto
   - Areas de expertise faltantes (seguranca, performance, arquitetura, etc.)

## Processo de Comparacao de Versoes

1. Para cada asset local que corresponda a item de collection, construa a URL raw do GitHub:
   - Agents: `https://raw.githubusercontent.com/github/awesome-copilot/main/agents/<filename>`
   - Prompts: `https://raw.githubusercontent.com/github/awesome-copilot/main/prompts/<filename>`
   - Instructions: `https://raw.githubusercontent.com/github/awesome-copilot/main/instructions/<filename>`
2. Busque a versao remota usando a tool `#fetch`
3. Compare o conteudo completo do arquivo (incluindo front matter e corpo)
4. Identifique diferencas especificas:
   - **Mudancas de front matter** (description, tools, applyTo patterns)
   - **Modificacoes no array de tools** (adicoes, remocoes, renomeacoes)
   - **Atualizacoes de conteudo** (instrucoes, exemplos, guidelines)
5. Documente diferencas-chave para assets desatualizados
6. Calcule similaridade para determinar se update e necessario

## Processo de Download de Assets de Collection

Quando o usuario confirmar a instalacao de uma collection:

1. **Fetch Collection Manifest**: Baixe o YAML da collection no repositorio awesome-copilot
2. **Download Individual Assets**: Para cada item:
   - Baixe conteudo raw do GitHub
   - Valide formato de arquivo e estrutura de front matter
   - Cheque compliance com convencoes de nomenclatura
3. **Install to Appropriate Directories**:
   - `*.prompt.md` → `prompts/`
   - `*.instructions.md` → `instructions/`
   - `*.agent.md` → `agents/`
4. **Avoid Duplicates**: Pule arquivos substancialmente similares aos assets existentes
5. **Report Installation**: Forneca resumo dos assets instalados e instrucoes de uso

## Requisitos

- Use a tool `fetch` para obter dados de collections do repositorio awesome-copilot
- Use a tool `githubRepo` para obter conteudo de assets individuais para download
- Scanne o file system local para assets existentes em `prompts/`, `instructions/` e `agents/`
- Leia front matter YAML de assets locais para extrair descricoes e capacidades
- Compare collections com o contexto do repositorio para identificar matches relevantes
- Foque em collections que preenchem gaps de capacidade em vez de duplicar assets existentes
- Valide que collections sugeridas alinham com stack e necessidades do repositorio
- Forneca rationale claro para cada sugestao com beneficios especificos
- Permita download e instalacao automatica de assets para os diretorios apropriados
- Garanta que assets baixados sigam convencoes de nomenclatura e formatacao
- Forneca orientacao de uso explicando como as collections melhoram o workflow
- Inclua links para collections e assets individuais

## Workflow de Instalacao de Collection

1. **User Confirms Collection**: Usuario seleciona collections para instalacao
2. **Fetch Collection Manifest**: Baixar manifest YAML do repositorio awesome-copilot
3. **Asset Download Loop**: Para cada asset na collection:
   - Baixar conteudo raw do GitHub
   - Validar formato e estrutura
   - Checar overlap substancial com assets locais
   - Instalar no diretorio apropriado (`prompts/`, `instructions/`, `agents/`)
4. **Installation Summary**: Reportar assets instalados com instrucoes de uso
5. **Workflow Enhancement Guide**: Explicar como a collection melhora as capacidades de desenvolvimento

## Orientacao Pos-Instalacao

Depois de instalar uma collection, forneca:
- **Asset Overview**: Lista de prompts, instructions e chat modes instalados
- **Usage Examples**: Como ativar e usar cada tipo de asset
- **Workflow Integration**: Boas praticas para incorporar assets ao processo
- **Customization Tips**: Como modificar assets para necessidades do projeto
- **Related Collections**: Sugestoes de collections complementares

## Referencia de Icones

- ✅ Collection recomendada / Asset up-to-date
- ⚠️ Collection com algum overlap mas ainda valiosa
- ❌ Collection nao recomendada (overlap significativo ou irrelevante)
- 🎯 Collection de alto valor que preenche gaps importantes
- 📁 Collection parcialmente instalada (alguns assets pulados por duplicatas)
- 🔄 Asset desatualizado (update disponivel)

## Tratamento de Updates

Quando assets desatualizados forem identificados:
1. Inclua-os na analise com status 🔄
2. Documente diferencas especificas para cada asset desatualizado
3. Forneca recomendacao de update com mudancas-chave
4. Quando o usuario pedir update, substitua o arquivo local inteiro pela versao remota
5. Preserve a localizacao do arquivo no diretorio apropriado (`agents/`, `prompts/`, ou `instructions/`)
