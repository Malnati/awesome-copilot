---
agent: 'agent'
description: 'Analise recursos Azure usados no app (arquivos IaC e/ou recursos em um resource group alvo) e otimize custos - criando issues no GitHub para otimizações identificadas.'
---

## Otimização de Custos Azure

Este fluxo analisa arquivos Infrastructure-as-Code (IaC) e recursos Azure para gerar recomendações de otimização de custos. Cria issues individuais no GitHub para cada oportunidade de otimização e uma issue EPIC para coordenar a implementação, permitindo rastreamento e execução eficiente das iniciativas de economia.

## Pré-requisitos
- MCP server Azure configurado e autenticado
- MCP server GitHub configurado e autenticado
- Repositório GitHub alvo identificado
- Recursos Azure implantados (arquivos IaC opcionais, mas úteis)
- Prefira ferramentas MCP Azure (`azmcp-*`) ao invés do Azure CLI direto quando disponível

## Etapas do Fluxo

### Etapa 1: Obter Melhores Práticas Azure
**Ação**: Recupere melhores práticas de otimização de custos antes da análise
**Ferramentas**: Ferramenta de melhores práticas MCP Azure
**Processo**:
1. **Carregar Melhores Práticas**:
   - Execute `azmcp-bestpractices-get` para obter algumas das diretrizes mais recentes de otimização Azure. Isso pode não cobrir todos os cenários, mas fornece uma base.
   - Use essas práticas para informar a análise e recomendações subsequentes o máximo possível
   - Referencie melhores práticas nas recomendações de otimização, seja do output da ferramenta MCP ou documentação geral Azure

### Etapa 2: Descobrir Infraestrutura Azure
**Ação**: Descubra e analise dinamicamente recursos e configurações Azure
**Ferramentas**: Ferramentas MCP Azure + fallback Azure CLI + acesso ao sistema de arquivos local
**Process**:
1. **Resource Discovery**:
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

2. **IaC Detection**:
   - Use `file_search` to scan for IaC files: "**/*.bicep", "**/*.tf", "**/main.json", "**/*template*.json"
   - Parse resource definitions to understand intended configurations
   - Compare against discovered resources to identify discrepancies
   - Note presence of IaC files for implementation recommendations later on
   - Do NOT use any other file from the repository, only IaC files. Using other files is NOT allowed as it is not a source of truth.
   - If you do not find IaC files, then STOP and report no IaC files found to the user.

3. **Configuration Analysis**:
   - Extract current SKUs, tiers, and settings for each resource
   - Identify resource relationships and dependencies
   - Map resource utilization patterns where available

### Step 3: Collect Usage Metrics & Validate Current Costs
**Action**: Gather utilization data AND verify actual resource costs
**Tools**: Azure MCP monitoring tools + Azure CLI
**Process**:
1. **Find Monitoring Sources**:
   - Use `azmcp-monitor-workspace-list --subscription <id>` to find Log Analytics workspaces
   - Use `azmcp-monitor-table-list --subscription <id> --workspace <name> --table-type "CustomLog"` to discover available data

2. **Execute Usage Queries**:
   - Use `azmcp-monitor-log-query` with these predefined queries:
     - Query: "recent" for recent activity patterns
     - Query: "errors" for error-level logs indicating issues
   - For custom analysis, use KQL queries:
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

3. **Calculate Baseline Metrics**:
   - CPU/Memory utilization averages
   - Database throughput patterns
   - Storage access frequency
   - Function execution rates

4. **VALIDATE CURRENT COSTS**:
   - Using the SKU/tier configurations discovered in Step 2
   - Look up current Azure pricing at https://azure.microsoft.com/pricing/ or use `az billing` commands
   - Document: Resource → Current SKU → Estimated monthly cost
   - Calculate realistic current monthly total before proceeding to recommendations

### Step 4: Generate Cost Optimization Recommendations
**Action**: Analyze resources to identify optimization opportunities
**Tools**: Local analysis using collected data
**Process**:
1. **Apply Optimization Patterns** based on resource types found:

   **Compute Optimizations**:
   - App Service Plans: Right-size based on CPU/memory usage
   - Function Apps: Premium → Consumption plan for low usage
   - Virtual Machines: Scale down oversized instances

   **Database Optimizations**:
   - Cosmos DB:
     - Provisioned → Serverless for variable workloads
     - Right-size RU/s based on actual usage
   - SQL Database: Right-size service tiers based on DTU usage

   **Storage Optimizations**:
   - Implement lifecycle policies (Hot → Cool → Archive)
   - Consolidate redundant storage accounts
   - Right-size storage tiers based on access patterns

   **Infrastructure Optimizations**:
   - Remove unused/redundant resources
   - Implement auto-scaling where beneficial
   - Schedule non-production environments

2. **Calculate Evidence-Based Savings**:
   - Current validated cost → Target cost = Savings
   - Document pricing source for both current and target configurations

3. **Calculate Priority Score** for each recommendation:
   ```
   Priority Score = (Value Score × Monthly Savings) / (Risk Score × Implementation Days)

   High Priority: Score > 20
   Medium Priority: Score 5-20
   Low Priority: Score < 5
   ```

4. **Validate Recommendations**:
   - Ensure Azure CLI commands are accurate
   - Verify estimated savings calculations
   - Assess implementation risks and prerequisites
   - Ensure all savings calculations have supporting evidence

### Step 5: User Confirmation
**Action**: Present summary and get approval before creating GitHub issues
**Process**:
1. **Display Optimization Summary**:
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

2. **Wait for User Confirmation**: Only proceed if user confirms

### Step 6: Create Individual Optimization Issues
**Action**: Create separate GitHub issues for each optimization opportunity. Label them with "cost-optimization" (green color), "azure" (blue color).
**MCP Tools Required**: `create_issue` for each recommendation
**Process**:
1. **Create Individual Issues** using this template:

   **Title Format**: `[COST-OPT] [Resource Type] - [Brief Description] - $X/month savings`

   **Body Template**:
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

### Step 7: Create EPIC Coordinating Issue
**Action**: Create master issue to track all optimization work. Label it with "cost-optimization" (green color), "azure" (blue color), and "epic" (purple color).
**MCP Tools Required**: `create_issue` for EPIC
**Note about mermaid diagrams**: Ensure you verify mermaid syntax is correct and create the diagrams taking accessibility guidelines into account (styling, colors, etc.).
**Process**:
1. **Create EPIC Issue**:

   **Title**: `[EPIC] Azure Cost Optimization Initiative - $X/month potential savings`

   **Body Template**:
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
