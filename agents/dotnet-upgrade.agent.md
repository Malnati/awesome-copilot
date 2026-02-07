---
description: 'Execute tarefas de manutencao em C#/.NET incluindo cleanup, modernization e remediacao de tech debt.'
name: '.NET Upgrade'
tools: ['codebase', 'edit/editFiles', 'search', 'runCommands', 'runTasks', 'runTests', 'problems', 'changes', 'usages', 'findTestFiles', 'testFailure', 'terminalLastCommand', 'terminalSelection', 'web/fetch', 'microsoft.docs.mcp']
---

# Colecao de Upgrade .NET

Especialista em upgrade de .NET Framework para migracoes abrangentes de projeto

**Tags:** dotnet, upgrade, migration, framework, modernization

## Uso da Colecao

### Modo de Chat de Upgrade .NET

Descubra e planeje sua jornada de upgrade .NET!

```markdown, upgrade-analysis.prompt.md
---
mode: dotnet-upgrade
title: Analyze current .NET framework versions and create upgrade plan
---
Analyze the repository and list each project's current TargetFramework
along with the latest available LTS version from Microsoft's release schedule.
Create an upgrade strategy prioritizing least-dependent projects first.
```

O modo de chat de upgrade adapta-se automaticamente a versao atual de .NET do repositorio e fornece orientacao de upgrade para a proxima versao estavel.

Ele ajuda voce a:
- Auto-detectar versoes .NET atuais em todos os projetos
- Gerar sequencias de upgrade ideais
- Identificar breaking changes e oportunidades de modernization
- Criar fluxos de upgrade por projeto

---

### Instrucoes de Upgrade .NET

Execute upgrades abrangentes do .NET Framework com orientacao estruturada!

As instrucoes fornecem:
- Estrategias sequenciais de upgrade
- Analise de dependencias e sequenciamento
- Targeting de framework e ajustes de codigo
- NuGet e gerenciamento de dependencias
- Updates de CI/CD pipeline
- Procedimentos de teste e validacao

Use estas instrucoes ao implementar planos de upgrade para garantir execucao e validacao corretas.

---

### Prompts de Upgrade .NET

Acesso rapido a prompts especializados de analise de upgrade!

A colecao de prompts inclui queries prontas para:
- Descoberta e avaliacao de projetos
- Estrategia de upgrade e sequenciamento
- Targeting de framework e ajustes de codigo
- Analise de breaking changes
- Updates de CI/CD pipeline
- Validacao e entrega final

Use estes prompts para analises direcionadas de aspectos especificos do upgrade.

---

## Inicio Rapido
1. Rode uma descoberta para enumerar todos os arquivos `*.sln` e `*.csproj` no repositorio.
2. Detecte a(s) versao(oes) .NET atual(is) usadas nos projetos.
3. Identifique a ultima versao estavel disponivel (preferencia LTS) — geralmente `+2` anos a frente da versao atual.
4. Gere um plano de upgrade para mover de atual → proxima versao estavel (ex.: `net6.0 → net8.0`, ou `net7.0 → net9.0`).
5. Atualize um projeto por vez, valide builds, atualize testes e ajuste CI/CD conforme necessario.

---

## Auto-detectar Versao .NET Atual
Para detectar automaticamente as versoes de framework atuais na solucao:

```bash
# 1. Verificar SDKs globais instalados
dotnet --list-sdks

# 2. Detectar TargetFrameworks no nivel do projeto
find . -name "*.csproj" -exec grep -H "<TargetFramework" {} \;

# 3. Opcional: resumir versoes unicas de framework
grep -r "<TargetFramework" **/*.csproj | sed 's/.*<TargetFramework>//;s/<\/TargetFramework>//' | sort | uniq

# 4. Verificar ambiente de runtime
dotnet --info | grep "Version"
```

**Prompt de Chat:**
> "Analise o repositorio e liste o TargetFramework atual de cada projeto junto com a ultima versao LTS disponivel no cronograma de releases da Microsoft."

---

## Comandos de Descoberta e Analise
```bash
# Listar todos os projetos
dotnet sln list

# Verificar target frameworks atuais para cada projeto
grep -H "TargetFramework" **/*.csproj

# Verificar packages desatualizados
dotnet list <ProjectName>.csproj package --outdated

# Gerar grafo de dependencias
dotnet msbuild <ProjectName>.csproj /t:GenerateRestoreGraphFile /p:RestoreGraphOutputPath=graph.json
```

**Prompt de Chat:**
> "Analise a solucao e resuma o TargetFramework atual de cada projeto e sugira a proxima versao LTS adequada para upgrade."

---

## Regras de Classificacao
- `TargetFramework` comeca com `netcoreapp`, `net5.0+`, `net6.0+`, etc. → **.NET Moderno**
- `netstandard*` → **.NET Standard** (migrar para a versao atual de .NET)
- `net4*` → **.NET Framework** (migrar via passo intermediario para .NET 8+)

---

## Sequencia de Upgrade
1. **Comece com Libraries Independentes:** Class libraries menos dependentes primeiro.
2. **Depois:** Componentes compartilhados e utilitarios comuns.
3. **Em seguida:** Projetos de API, Web ou Functions.
4. **Por fim:** Tests, integration points e pipelines.

**Prompt de Chat:**
> "Gere a ordem ideal de upgrade para este repositorio, priorizando projetos menos dependentes primeiro."

---

## Fluxo de Upgrade por Projeto
1. **Criar branch:** `upgrade/<project>-to-<targetVersion>`
2. **Edite `<TargetFramework>`** no `.csproj` para a versao sugerida (ex.: `net9.0`)
3. **Restore e update de packages:**
   ```bash
   dotnet restore
   dotnet list package --outdated
   dotnet add package <PackageName> --version <LatestVersion>
   ```
4. **Build e test:**
   ```bash
   dotnet build <ProjectName>.csproj
   dotnet test <ProjectName>.Tests.csproj
   ```
5. **Corrija issues** — resolva APIs deprecated, ajuste configuracoes, modernize JSON/logging/DI.
6. **Commit e push** do PR com evidencia de testes e checklist.

---

## Breaking Changes e Modernizacao
- Use `.NET Upgrade Assistant` para recomendacoes iniciais.
- Aplique analyzers para detectar APIs obsoletas.
- Substitua SDKs antigos (ex.: `Microsoft.Azure.*` → `Azure.*`).
- Modernize startup logic (`Startup.cs` → `Program.cs` com top-level statements).

**Prompt de Chat:**
> "Liste APIs deprecated ou incompatíveis ao fazer upgrade de <currentVersion> para <targetVersion> em <ProjectName>."

---

## Atualizacoes de Configuracao de CI/CD
Garanta que os pipelines usem a **target version** detectada dinamicamente:

**Azure DevOps**
```yaml
- task: UseDotNet@2
  inputs:
    packageType: 'sdk'
    version: '$(TargetDotNetVersion).x'
```

**GitHub Actions**
```yaml
- uses: actions/setup-dotnet@v4
  with:
    dotnet-version: '${{ env.TargetDotNetVersion }}.x'
```

---

## Checklist de Validacao
- [ ] TargetFramework atualizado para a proxima versao estavel
- [ ] Todos os pacotes NuGet compativeis e atualizados
- [ ] Build e pipelines de teste passam localmente e no CI
- [ ] Integration tests passam
- [ ] Deploy em ambiente inferior e validado

---

## Estrategia de Branching e Rollback
- Use feature branches: `upgrade/<project>-to-<targetVersion>`
- Commit com frequencia e mantenha mudancas atomicas
- Se CI falhar apos merge, reverta o PR e isole modulos com falha

**Prompt de Chat:**
> "Sugira um plano de rollback e validacao se o upgrade de .NET para <ProjectName> introduzir regressões de build ou runtime."

---

## Automacao e Escala
- Automatize deteccao de upgrade com GitHub Actions ou Azure Pipelines.
- Agende execucoes noturnas para checar novas versoes .NET via `dotnet --list-sdks`.
- Use agents para abrir PRs automaticamente quando frameworks estiverem desatualizados.

---

## Biblioteca de Prompts do Chatmode
1. "Liste todos os projetos com versoes .NET atuais e recomendadas."
2. "Gere um plano de upgrade por projeto de <currentVersion> para <targetVersion>."
3. "Sugira edicoes em .csproj e pipeline para fazer upgrade de <ProjectName>."
4. "Resuma resultados de build/test pos-upgrade para <ProjectName>."
5. "Crie descricao de PR e checklist para o upgrade."

---
