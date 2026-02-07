---
description: 'Execute tarefas de manutencao em C#/.NET incluindo cleanup, modernization e remediacao de tech debt.'
name: '.NET Upgrade'
tools: ['codebase', 'edit/editFiles', 'search', 'runCommands', 'runTasks', 'runTests', 'problems', 'changes', 'usages', 'findTestFiles', 'testFailure', 'terminalLastCommand', 'terminalSelection', 'web/fetch', 'microsoft.docs.mcp']
---

# .NET Upgrade Collection

Especialista em upgrade de .NET Framework para migracoes abrangentes de projeto

**Tags:** dotnet, upgrade, migration, framework, modernization

## Collection Usage

### .NET Upgrade Chat Mode

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

### .NET Upgrade Instructions

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

### .NET Upgrade Prompts

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

## Quick Start
1. Rode uma descoberta para enumerar todos os arquivos `*.sln` e `*.csproj` no repositorio.
2. Detecte a(s) versao(oes) .NET atual(is) usadas nos projetos.
3. Identifique a ultima versao estavel disponivel (preferencia LTS) — geralmente `+2` anos a frente da versao atual.
4. Gere um plano de upgrade para mover de atual → proxima versao estavel (ex.: `net6.0 → net8.0`, ou `net7.0 → net9.0`).
5. Atualize um projeto por vez, valide builds, atualize testes e ajuste CI/CD conforme necessario.

---

## Auto-Detect Current .NET Version
Para detectar automaticamente as versoes de framework atuais na solucao:

```bash
# 1. Check global SDKs installed
dotnet --list-sdks

# 2. Detect project-level TargetFrameworks
find . -name "*.csproj" -exec grep -H "<TargetFramework" {} \;

# 3. Optional: summarize unique framework versions
grep -r "<TargetFramework" **/*.csproj | sed 's/.*<TargetFramework>//;s/<\/TargetFramework>//' | sort | uniq

# 4. Verify runtime environment
dotnet --info | grep "Version"
```

**Chat Prompt:**
> "Analyze the repository and list each project’s current TargetFramework along with the latest available LTS version from Microsoft’s release schedule."

---

## Discovery & Analysis Commands
```bash
# List all projects
dotnet sln list

# Check current target frameworks for each project
grep -H "TargetFramework" **/*.csproj

# Check outdated packages
dotnet list <ProjectName>.csproj package --outdated

# Generate dependency graph
dotnet msbuild <ProjectName>.csproj /t:GenerateRestoreGraphFile /p:RestoreGraphOutputPath=graph.json
```

**Chat Prompt:**
> "Analyze the solution and summarize each project’s current TargetFramework and suggest the appropriate next LTS upgrade version."

---

## Classification Rules
- `TargetFramework` comeca com `netcoreapp`, `net5.0+`, `net6.0+`, etc. → **Modern .NET**
- `netstandard*` → **.NET Standard** (migrar para a versao atual de .NET)
- `net4*` → **.NET Framework** (migrar via passo intermediario para .NET 8+)

---

## Upgrade Sequence
1. **Comece com Libraries Independentes:** Class libraries menos dependentes primeiro.
2. **Depois:** Componentes compartilhados e utilitarios comuns.
3. **Em seguida:** Projetos de API, Web ou Functions.
4. **Por fim:** Tests, integration points e pipelines.

**Chat Prompt:**
> "Generate the optimal upgrade order for this repository, prioritizing least-dependent projects first."

---

## Per-Project Upgrade Flow
1. **Create branch:** `upgrade/<project>-to-<targetVersion>`
2. **Edite `<TargetFramework>`** no `.csproj` para a versao sugerida (ex.: `net9.0`)
3. **Restore & update packages:**
   ```bash
   dotnet restore
   dotnet list package --outdated
   dotnet add package <PackageName> --version <LatestVersion>
   ```
4. **Build & test:**
   ```bash
   dotnet build <ProjectName>.csproj
   dotnet test <ProjectName>.Tests.csproj
   ```
5. **Corrija issues** — resolva APIs deprecated, ajuste configuracoes, modernize JSON/logging/DI.
6. **Commit & push** PR com evidencia de testes e checklist.

---

## Breaking Changes & Modernization
- Use `.NET Upgrade Assistant` para recomendacoes iniciais.
- Aplique analyzers para detectar APIs obsoletas.
- Substitua SDKs antigos (ex.: `Microsoft.Azure.*` → `Azure.*`).
- Modernize startup logic (`Startup.cs` → `Program.cs` com top-level statements).

**Chat Prompt:**
> "List deprecated or incompatible APIs when upgrading from <currentVersion> to <targetVersion> for <ProjectName>."

---

## CI/CD Configuration Updates
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

## Validation Checklist
- [ ] TargetFramework atualizado para a proxima versao estavel
- [ ] Todos os pacotes NuGet compativeis e atualizados
- [ ] Build e pipelines de teste passam localmente e no CI
- [ ] Integration tests passam
- [ ] Deploy em ambiente inferior e validado

---

## Branching & Rollback Strategy
- Use feature branches: `upgrade/<project>-to-<targetVersion>`
- Commit com frequencia e mantenha mudancas atomicas
- Se CI falhar apos merge, reverta o PR e isole modulos com falha

**Chat Prompt:**
> "Suggest a rollback and validation plan if the .NET upgrade for <ProjectName> introduces build or runtime regressions."

---

## Automation & Scaling
- Automatize deteccao de upgrade com GitHub Actions ou Azure Pipelines.
- Agende execucoes noturnas para checar novas versoes .NET via `dotnet --list-sdks`.
- Use agents para abrir PRs automaticamente quando frameworks estiverem desatualizados.

---

## Chatmode Prompt Library
1. "List all projects with current and recommended .NET versions."
2. "Generate a per-project upgrade plan from <currentVersion> to <targetVersion>."
3. "Suggest .csproj and pipeline edits to upgrade <ProjectName>."
4. "Summarize build/test results post-upgrade for <ProjectName>."
5. "Create PR description and checklist for the upgrade."

---
