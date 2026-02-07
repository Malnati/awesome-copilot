---
agent: 'agent'
description: 'Analise recursos Azure usados no app (arquivos IaC e/ou recursos em um resource group alvo) e otimize custos - criando issues no GitHub para otimizações identificadas.'
---

## Otimizacao de Custos Azure

Este fluxo analisa arquivos Infrastructure-as-Code (IaC) e recursos Azure para gerar recomendacoes de otimizacao de custos. Cria issues individuais no GitHub para cada oportunidade de otimizacao e uma issue EPIC para coordenar a implementacao, permitindo rastreamento e execucao eficiente das iniciativas de economia.

## Pre-requisitos
- MCP server Azure configurado e autenticado
- MCP server GitHub configurado e autenticado
- Repositorio GitHub alvo identificado
- Recursos Azure implantados (arquivos IaC opcionais, mas uteis)
- Prefira ferramentas MCP Azure (`azmcp-*`) ao inves do Azure CLI direto quando disponivel

## Etapas do Fluxo

### Etapa 1: Obter Melhores Praticas Azure
**Acao**: Recupere melhores praticas de otimizacao de custos antes da analise
**Ferramentas**: Ferramenta de melhores praticas MCP Azure
**Processo**:
1. **Carregar Melhores Praticas**:
   - Execute `azmcp-bestpractices-get` para obter algumas das diretrizes mais recentes de otimizacao Azure. Isso pode nao cobrir todos os cenarios, mas fornece uma base.
   - Use essas praticas para informar a analise e recomendacoes subsequentes o maximo possivel
   - Referencie melhores praticas nas recomendacoes de otimizacao, seja do output da ferramenta MCP ou documentacao geral Azure

### Etapa 2: Descobrir Infraestrutura Azure
**Acao**: Descubra e analise dinamicamente recursos e configuracoes Azure
**Ferramentas**: Ferramentas MCP Azure + fallback Azure CLI + acesso ao sistema de arquivos local
**Processo**:
1. **Descoberta de Recursos**:
   - Execute `azmcp-subscription-list` to find available subscriptions
   - Execute `azmcp-group-list --subscription <subscription-id>` to find resource groups
   - Get a list of all resources in the relevant group(s):
     - Use `az resource list --subscription <id> --resource-group <name>`
   - For each resource type, use MCP tools first if possible, then CLI fallback:
     - `azmcp-cosmos-account-list --subscription <id>` - Cosmos DB accounts
     - `azmcp-storage-account-list --subscription <id>` - Storage accounts
     - `azmcp-monitor-workspace-list --subscription <id>` - Log Analytics workspaces
     - `azmcp-keyvault-key-list` - Key Vaults
     - `az webapp list` - Web Apps (fallback - no MCP tool available)
     - `az appservice plan list` - App Service Plans (fallback)
     - `az functionapp list` - Function Apps (fallback)
     - `az sql server list` - SQL Servers (fallback)
     - `az redis list` - Redis Cache (fallback)
     - ... and so on for other resource types

2. **Deteccao de IaC**:
   - Use `file_search` para procurar arquivos IaC: "**/*.bicep", "**/*.tf", "**/main.json", "**/*template*.json"
   - Analise definicoes de recursos para entender configuracoes pretendidas
   - Compare com recursos descobertos para identificar discrepancias
   - Observe a presenca de arquivos IaC para recomendacoes de implementacao depois
   - Do NOT use any other file from the repository, only IaC files. Using other files is NOT allowed as it is not a source of truth.
   - If you do not find IaC files, then STOP and report no IaC files found to the user.

3. **Analise de Configuracao**:
   - Extraia SKUs, tiers e configuracoes atuais de cada recurso
   - Identifique relacionamentos e dependencias entre recursos
   - Mapeie padroes de utilizacao quando disponivel

### Etapa 3: Coletar Metricas de Uso e Validar Custos Atuais
**Acao**: Coletar dados de utilizacao E validar custos reais de recursos
**Ferramentas**: Ferramentas de monitoramento MCP Azure + Azure CLI
**Processo**:
1. **Encontrar Fontes de Monitoramento**:
   - Use `azmcp-monitor-workspace-list --subscription <id>` para encontrar Log Analytics workspaces
   - Use `azmcp-monitor-table-list --subscription <id> --workspace <name> --table-type "CustomLog"` para descobrir dados disponiveis

2. **Executar Queries de Uso**:
   - Use `azmcp-monitor-log-query` com estas queries predefinidas:
     - Query: "recent" para padroes de atividade recente
     - Query: "errors" para logs de nivel erro indicando problemas
   - Para analises custom, use queries KQL:
   ```kql
   // CPU utilization for App Services
   AppServiceAppLogs
   | where TimeGenerated > ago(7d)
   | summarize avg(CpuTime) by Resource, bin(TimeGenerated, 1h)

   // Cosmos DB RU consumption
   AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB"
   | where TimeGenerated > ago(7d)
   | summarize avg(RequestCharge) by Resource

   // Storage account access patterns
   StorageBlobLogs
   | where TimeGenerated > ago(7d)
   | summarize RequestCount=count() by AccountName, bin(TimeGenerated, 1d)
   ```

3. **Calcular Metricas Base**:
   - Medias de utilizacao de CPU/Memoria
   - Padroes de throughput de banco de dados
   - Frequencia de acesso a storage
   - Taxas de execucao de functions

4. **VALIDAR CUSTOS ATUAIS**:
   - Usando as configuracoes de SKU/tier descobertas na Etapa 2
   - Consulte precos atuais do Azure em https://azure.microsoft.com/pricing/ ou use comandos `az billing`
   - Documente: Recurso → SKU Atual → Custo mensal estimado
   - Calcule o total mensal atual realista antes de prosseguir com recomendacoes

### Etapa 4: Gerar Recomendacoes de Otimizacao de Custos
**Acao**: Analisar recursos para identificar oportunidades de otimizacao
**Ferramentas**: Analise local usando dados coletados
**Processo**:
1. **Aplicar Padroes de Otimizacao** com base nos tipos de recurso encontrados:

   **Compute Optimizations**:
   - App Service Plans: Right-size baseado em uso de CPU/memoria
   - Function Apps: Premium → Consumption plan para baixo uso
   - Virtual Machines: Scale down instancias superdimensionadas

   **Database Optimizations**:
   - Cosmos DB:
     - Provisioned → Serverless para cargas variaveis
     - Right-size RU/s baseado em uso real
   - SQL Database: Right-size tiers de servico com base no uso de DTU

   **Storage Optimizations**:
   - Implementar lifecycle policies (Hot → Cool → Archive)
   - Consolidar storage accounts redundantes
   - Right-size storage tiers baseado em padroes de acesso

   **Infrastructure Optimizations**:
   - Remover recursos nao utilizados/redundantes
   - Implementar auto-scaling quando benefico
   - Agendar ambientes nao produtivos

2. **Calcular Economias Baseadas em Evidencia**:
   - Custo atual validado → Custo alvo = Economia
   - Documente a fonte de preco para as configuracoes atual e alvo

3. **Calcular Priority Score** para cada recomendacao:
   ```
   Priority Score = (Value Score × Monthly Savings) / (Risk Score × Implementation Days)

   High Priority: Score > 20
   Medium Priority: Score 5-20
   Low Priority: Score < 5
   ```

4. **Validar Recomendacoes**:
   - Garanta que comandos Azure CLI estao corretos
   - Verifique os calculos de economia estimada
   - Avalie riscos e pre-requisitos de implementacao
   - Garanta que todos os calculos de economia tenham evidencia de suporte

### Etapa 5: Confirmacao do Usuario
**Acao**: Apresente o resumo e obtenha aprovacao antes de criar issues no GitHub
**Processo**:
1. **Exibir Resumo de Otimizacao**:
   ```
   🎯 Azure Cost Optimization Summary

   📊 Analysis Results:
   • Total Resources Analyzed: X
   • Current Monthly Cost: $X
   • Potential Monthly Savings: $Y
   • Optimization Opportunities: Z
   • High Priority Items: N

   🏆 Recommendations:
   1. [Resource]: [Current SKU] → [Target SKU] = $X/month savings - [Risk Level] | [Implementation Effort]
   2. [Resource]: [Current Config] → [Target Config] = $Y/month savings - [Risk Level] | [Implementation Effort]
   3. [Resource]: [Current Config] → [Target Config] = $Z/month savings - [Risk Level] | [Implementation Effort]
   ... and so on

   💡 This will create:
   • Y individual GitHub issues (one per optimization)
   • 1 EPIC issue to coordinate implementation

   ❓ Proceed with creating GitHub issues? (y/n)
   ```

2. **Aguardar Confirmacao do Usuario**: Prossiga apenas se o usuario confirmar

### Etapa 6: Criar Issues Individuais de Otimizacao
**Acao**: Crie issues separadas no GitHub para cada oportunidade de otimizacao. Rotule com "cost-optimization" (cor verde) e "azure" (cor azul).
**Ferramentas MCP Necessarias**: `create_issue` para cada recomendacao
**Processo**:
1. **Criar Issues Individuais** usando este template:

   **Formato do Titulo**: `[COST-OPT] [Resource Type] - [Brief Description] - $X/month savings`

   **Template do Corpo**:
   ```markdown
   ## 💰 Cost Optimization: [Brief Title]

   **Monthly Savings**: $X | **Risk Level**: [Low/Medium/High] | **Implementation Effort**: X days

   ### 📋 Description
   [Clear explanation of the optimization and why it's needed]

   ### 🔧 Implementation

   **IaC Files Detected**: [Yes/No - based on file_search results]

   ```bash
   # If IaC files found: Show IaC modifications + deployment
   # File: infrastructure/bicep/modules/app-service.bicep
   # Change: sku.name: 'S3' → 'B2'
   az deployment group create --resource-group [rg] --template-file infrastructure/bicep/main.bicep

   # If no IaC files: Direct Azure CLI commands + warning
   # ⚠️ No IaC files found. If they exist elsewhere, modify those instead.
   az appservice plan update --name [plan] --sku B2
   ```

   ### 📊 Evidence
   - Current Configuration: [details]
   - Usage Pattern: [evidence from monitoring data]
   - Cost Impact: $X/month → $Y/month
   - Best Practice Alignment: [reference to Azure best practices if applicable]

   ### ✅ Validation Steps
   - [ ] Test in non-production environment
   - [ ] Verify no performance degradation
   - [ ] Confirm cost reduction in Azure Cost Management
   - [ ] Update monitoring and alerts if needed

   ### ⚠️ Risks & Considerations
   - [Risk 1 and mitigation]
   - [Risk 2 and mitigation]

   **Priority Score**: X | **Value**: X/10 | **Risk**: X/10
   ```

### Etapa 7: Criar Issue EPIC de Coordenacao
**Acao**: Crie uma issue master para rastrear todo o trabalho de otimizacao. Rotule com "cost-optimization" (cor verde), "azure" (cor azul) e "epic" (cor roxa).
**Ferramentas MCP Necessarias**: `create_issue` para a EPIC
**Nota sobre diagramas mermaid**: Garanta que a sintaxe mermaid esteja correta e crie os diagramas considerando diretrizes de acessibilidade (estilo, cores, etc.).
**Processo**:
1. **Criar Issue EPIC**:

   **Titulo**: `[EPIC] Azure Cost Optimization Initiative - $X/month potential savings`

   **Template do Corpo**:
   ```markdown
   # 🎯 Azure Cost Optimization EPIC

   **Total Potential Savings**: $X/month | **Implementation Timeline**: X weeks

   ## 📊 Executive Summary
   - **Resources Analyzed**: X
   - **Optimization Opportunities**: Y
   - **Total Monthly Savings Potential**: $X
   - **High Priority Items**: N

   ## 🏗️ Current Architecture Overview

   ```mermaid
   graph TB
       subgraph "Resource Group: [name]"
           [Generated architecture diagram showing current resources and costs]
       end
   ```

   ## 📋 Implementation Tracking

   ### 🚀 High Priority (Implement First)
   - [ ] #[issue-number]: [Title] - $X/month savings
   - [ ] #[issue-number]: [Title] - $X/month savings

   ### ⚡ Medium Priority
   - [ ] #[issue-number]: [Title] - $X/month savings
   - [ ] #[issue-number]: [Title] - $X/month savings

   ### 🔄 Low Priority (Nice to Have)
   - [ ] #[issue-number]: [Title] - $X/month savings

   ## 📈 Progress Tracking
   - **Completed**: 0 of Y optimizations
   - **Savings Realized**: $0 of $X/month
   - **Implementation Status**: Not Started

   ## 🎯 Success Criteria
   - [ ] All high-priority optimizations implemented
   - [ ] >80% of estimated savings realized
   - [ ] No performance degradation observed
   - [ ] Cost monitoring dashboard updated

   ## 📝 Notes
   - Review and update this EPIC as issues are completed
   - Monitor actual vs. estimated savings
   - Consider scheduling regular cost optimization reviews
   ```

## Error Handling
- **Cost Validation**: If savings estimates lack supporting evidence or seem inconsistent with Azure pricing, re-verify configurations and pricing sources before proceeding
- **Azure Authentication Failure**: Provide manual Azure CLI setup steps
- **No Resources Found**: Create informational issue about Azure resource deployment
- **GitHub Creation Failure**: Output formatted recommendations to console
- **Insufficient Usage Data**: Note limitations and provide configuration-based recommendations only

## Success Criteria
- ✅ All cost estimates verified against actual resource configurations and Azure pricing
- ✅ Individual issues created for each optimization (trackable and assignable)
- ✅ EPIC issue provides comprehensive coordination and tracking
- ✅ All recommendations include specific, executable Azure CLI commands
- ✅ Priority scoring enables ROI-focused implementation
- ✅ Architecture diagram accurately represents current state
- ✅ User confirmation prevents unwanted issue creation
