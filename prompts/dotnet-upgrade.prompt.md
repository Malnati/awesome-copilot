---
name: ".NET Upgrade Analysis Prompts"
description: "Prompts prontos para analise abrangente e execucao de upgrade de framework .NET"
---
  # Project Discovery & Assessment
  - name: "Project Classification Analysis"
    prompt: "Identifique todos os projetos na solution e classifique por tipo (`.NET Framework`, `.NET Core`, `.NET Standard`). Analise cada `.csproj` para `TargetFramework` atual e uso de SDK."

  - name: "Dependency Compatibility Review"
    prompt: "Revise dependencias externas e internas para compatibilidade com o framework. Determine a complexidade de upgrade com base na profundidade do grafo de dependencias."

  - name: "Legacy Package Detection"
    prompt: "Identifique projetos com `packages.config` legado que precisam migrar para `PackageReference`."

  # Upgrade Strategy & Sequencing
  - name: "Project Upgrade Ordering"
    prompt: "Recomende uma ordem de upgrade dos projetos do menos para o mais dependente. Sugira como isolar upgrades de class libraries antes de migracoes de API ou Azure Functions."

  - name: "Incremental Strategy Planning"
    prompt: "Proponha uma estrategia de upgrade incremental com checkpoints de rollback. Avalie o uso do **Upgrade Assistant** ou **upgrades manuais** com base na estrutura do projeto."

  - name: "Progress Tracking Setup"
    prompt: "Gere um checklist de upgrade para acompanhar prontidao de build, teste e deploy em todos os projetos."

  # Framework Targeting & Code Adjustments
  - name: "Target Framework Selection"
    prompt: "Sugira o `TargetFramework` correto para cada projeto (ex.: `net8.0`). Revise e atualize configuracoes de SDK/build depreciadas."

  - name: "Code Modernization Analysis"
    prompt: "Identifique patterns de codigo que precisam modernizacao (ex.: `WebHostBuilder` → `HostBuilder`). Sugira substituicoes para APIs .NET depreciadas e libraries de terceiros."

  - name: "Async Pattern Conversion"
    prompt: "Recomende a conversao de chamadas sincronas para async quando apropriado para melhorar performance e escalabilidade."

  # NuGet & Dependency Management
  - name: "Package Compatibility Analysis"
    prompt: "Analise pacotes NuGet desatualizados ou incompatíveis e sugira versoes compatíveis. Identifique libraries de terceiros sem suporte a .NET 8 e forneca caminhos de migracao."

  - name: "Shared Dependency Strategy"
    prompt: "Recomende estrategias para upgrades de dependencias compartilhadas entre projetos. Avalie pacotes legados e sugira alternativas em namespaces suportados pela Microsoft."

  - name: "Transitive Dependency Review"
    prompt: "Revise dependencias transitivas e possiveis conflitos de versao apos o upgrade. Sugira estrategias para resolver conflitos."

  # CI/CD & Build Pipeline Updates
  - name: "Pipeline Configuration Analysis"
    prompt: "Analise definicoes YAML de build para pinagem de versao de SDK e recomende updates. Sugira modificacoes para tasks `UseDotNet@2` e `NuGetToolInstaller`."

  - name: "Build Pipeline Modernization"
    prompt: "Gere snippets de pipeline atualizados para migracao .NET 8. Recomende builds de validacao em branches de feature antes do merge na main."

  - name: "CI Automation Enhancement"
    prompt: "Identifique oportunidades de automatizar verificacao de teste e build em pipelines de CI. Sugira estrategias para validacao de CI continua."

  # Testing & Validation
  - name: "Build Validation Strategy"
    prompt: "Proponha checks de validacao para garantir que a solution atualizada compile e rode com sucesso. Recomende execucao automatizada de testes unitarios e integrados pos-upgrade."

  - name: "Service Integration Verification"
    prompt: "Gere etapas de validacao para verificar logging, telemetria e conectividade de servicos. Sugira estrategias para verificar compatibilidade retroativa e comportamento em runtime."

  - name: "Deployment Readiness Check"
    prompt: "Recomende etapas de verificacao de deploy em UAT antes do rollout em producao. Crie cenarios de teste abrangentes para componentes atualizados."

  # Breaking Change Analysis
  - name: "API Deprecation Detection"
    prompt: "Identifique APIs depreciadas ou namespaces removidos entre versoes alvo. Sugira varredura automatizada com `.NET Upgrade Assistant` e API Analyzer."

  - name: "API Replacement Strategy"
    prompt: "Recomende APIs ou libraries substitutas para areas conhecidas de breaking changes. Revise mudancas de configuracao como refatoracao de `Startup.cs` → `Program.cs`."

  - name: "Regression Testing Focus"
    prompt: "Sugira cenarios de teste de regressao focados em endpoints ou servicos atualizados. Crie planos de teste para validacao de funcionalidades criticas."

  # Version Control & Commit Strategy
  - name: "Branching Strategy Planning"
    prompt: "Recomende estrategia de branch para upgrade seguro com capacidade de rollback. Gere templates de commit para upgrades parciais e completos."

  - name: "PR Structure Optimization"
    prompt: "Sugira boas praticas para criar PRs estruturados (`Upgrade to .NET [Version]`). Identifique estrategias de tagging para PRs com breaking changes."

  - name: "Code Review Guidelines"
    prompt: "Recomende areas de foco para peer review (build, teste, validacao de dependencias). Crie checklists para reviews de upgrade eficazes."

  # Documentation & Communication
  - name: "Upgrade Documentation Strategy"
    prompt: "Sugira como documentar a mudanca de framework de cada projeto no PR. Proponha geracao automatizada de release notes com resumo de upgrades e resultados de testes."

  - name: "Stakeholder Communication"
    prompt: "Recomende comunicacao de upgrades de versao e timelines de migracao para consumidores. Gere templates de documentacao para updates de dependencias e resultados de validacao."

  - name: "Progress Tracking Systems"
    prompt: "Sugira manter dashboard de upgrade ou checklist em markdown. Crie templates para acompanhar progresso de upgrade em multiplos projetos."

  # Tools & Automation
  - name: "Upgrade Tool Selection"
    prompt: "Recomende quando e como usar: `.NET Upgrade Assistant`, `dotnet list package --outdated`, `dotnet migrate`, e visualizacao de dependencias `graph.json`."

  - name: "Analysis Script Generation"
    prompt: "Gere scripts ou prompts para analisar grafos de dependencias antes do upgrade. Proponha prompts assistidos por IA para Copilot identificar issues de upgrade automaticamente."

  - name: "Multi-Repository Validation"
    prompt: "Sugira como validar output de automacao em multiplos repositorios. Crie workflows padronizados de validacao para upgrades em escala enterprise."

  # Final Validation & Delivery
  - name: "Final Solution Validation"
    prompt: "Gere etapas de validacao para confirmar que a solution final atualizada passa em todos os checks. Sugira verificacao de deploy em producao pos-upgrade."

  - name: "Deployment Readiness Confirmation"
    prompt: "Recomende gerar resultados finais de testes e artefatos de build. Crie checklist resumindo conclusao entre projetos (builds/tests/deploy)."

  - name: "Release Documentation"
    prompt: "Gere release note resumindo mudancas de framework e updates de CI/CD. Crie documentacao abrangente de resumo de upgrade."

---
