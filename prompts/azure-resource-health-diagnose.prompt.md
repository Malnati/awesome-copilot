---
agent: 'agent'
description: 'Analise a saúde de recursos Azure, diagnostique problemas a partir de logs e telemetria, e crie um plano de remediação para problemas identificados.'
---

## Saude de Recursos Azure e Diagnostico de Problemas

Este fluxo analisa um recurso Azure especifico para avaliar seu status de saude, diagnosticar possiveis problemas usando logs e dados de telemetria, e desenvolver um plano de remediacao abrangente para quaisquer problemas encontrados.

## Pre-requisitos
- MCP server Azure configurado e autenticado
- Recurso Azure alvo identificado (nome e opcionalmente resource group/subscription)
- O recurso deve estar implantado e rodando para gerar logs/telemetria
- Prefira ferramentas MCP Azure (`azmcp-*`) ao inves do Azure CLI direto quando disponivel

## Etapas do Fluxo

### Etapa 1: Obter Melhores Praticas Azure
**Acao**: Recupere melhores praticas de diagnostico e troubleshooting
**Ferramentas**: Ferramenta de melhores praticas MCP Azure
**Processo**:
1. **Carregar Melhores Praticas**:
   - Execute a ferramenta de melhores praticas Azure para obter diretrizes de diagnostico
   - Foque em monitoramento de saude, analise de logs e padroes de resolucao de problemas
   - Use essas praticas para informar a abordagem de diagnostico e recomendacoes de remediacao

### Etapa 2: Descoberta e Identificacao de Recurso
**Acao**: Localize e identifique o recurso Azure alvo
**Ferramentas**: Ferramentas MCP Azure + fallback Azure CLI
**Processo**:
1. **Resource Lookup**:
   - Se apenas o nome do recurso foi fornecido: procure em subscriptions usando `azmcp-subscription-list`
   - Use `az resource list --name <resource-name>` para encontrar recursos correspondentes
   - Se houver multiplas correspondencias, solicite que o usuario especifique subscription/resource group
   - Colete informacoes detalhadas do recurso:
     - Tipo de recurso e status atual
     - Location, tags e configuracao
     - Servicos associados e dependencias

2. **Resource Type Detection**:
   - Identifique o tipo de recurso para determinar a abordagem de diagnostico adequada:
     - **Web Apps/Function Apps**: Application logs, performance metrics, dependency tracking
     - **Virtual Machines**: System logs, performance counters, boot diagnostics
     - **Cosmos DB**: Request metrics, throttling, partition statistics
     - **Storage Accounts**: Access logs, performance metrics, availability
     - **SQL Database**: Query performance, connection logs, resource utilization
     - **Application Insights**: Application telemetry, exceptions, dependencies
     - **Key Vault**: Access logs, certificate status, secret usage
     - **Service Bus**: Message metrics, dead letter queues, throughput

### Etapa 3: Avaliacao de Status de Saude
**Acao**: Avaliar a saude e disponibilidade atual do recurso
**Ferramentas**: Ferramentas de monitoramento MCP Azure + Azure CLI
**Processo**:
1. **Basic Health Check**:
   - Verifique o estado de provisionamento e status operacional do recurso
   - Valide disponibilidade e responsividade do servico
   - Revise mudanças recentes de deploy ou configuracao
   - Avalie utilizacao atual de recursos (CPU, memoria, storage, etc.)

2. **Service-Specific Health Indicators**:
   - **Web Apps**: HTTP response codes, response times, uptime
   - **Databases**: Connection success rate, query performance, deadlocks
   - **Storage**: Availability percentage, request success rate, latency
   - **VMs**: Boot diagnostics, guest OS metrics, network connectivity
   - **Functions**: Execution success rate, duration, error frequency

### Etapa 4: Analise de Logs e Telemetria
**Acao**: Analisar logs e telemetria para identificar problemas e padroes
**Ferramentas**: Ferramentas de monitoramento MCP Azure para queries no Log Analytics
**Processo**:
1. **Encontrar Fontes de Monitoramento**:
   - Use `azmcp-monitor-workspace-list` para identificar Log Analytics workspaces
   - Localize instancias de Application Insights associadas ao recurso
   - Identifique tabelas de log relevantes usando `azmcp-monitor-table-list`

2. **Executar Queries de Diagnostico**:
   Use `azmcp-monitor-log-query` com queries KQL direcionadas por tipo de recurso:

   **Analise Geral de Erros**:
   ```kql
   // Recent errors and exceptions
   union isfuzzy=true
       AzureDiagnostics,
       AppServiceHTTPLogs,
       AppServiceAppLogs,
       AzureActivity
   | where TimeGenerated > ago(24h)
   | where Level == "Error" or ResultType != "Success"
   | summarize ErrorCount=count() by Resource, ResultType, bin(TimeGenerated, 1h)
   | order by TimeGenerated desc
   ```

   **Analise de Performance**:
   ```kql
   // Performance degradation patterns
   Perf
   | where TimeGenerated > ago(7d)
   | where ObjectName == "Processor" and CounterName == "% Processor Time"
   | summarize avg(CounterValue) by Computer, bin(TimeGenerated, 1h)
   | where avg_CounterValue > 80
   ```

   **Queries Especificas de Aplicacao**:
   ```kql
   // Application Insights - Failed requests
   requests
   | where timestamp > ago(24h)
   | where success == false
   | summarize FailureCount=count() by resultCode, bin(timestamp, 1h)
   | order by timestamp desc

   // Database - Connection failures
   AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.SQL"
   | where Category == "SQLSecurityAuditEvents"
   | where action_name_s == "CONNECTION_FAILED"
   | summarize ConnectionFailures=count() by bin(TimeGenerated, 1h)
   ```

3. **Pattern Recognition**:
   - Identifique padroes recorrentes de erro ou anomalias
   - Correlacione erros com tempos de deploy ou mudancas de configuracao
   - Analise tendencias de performance e degradacao
   - Procure falhas de dependencia ou problemas em servicos externos

### Etapa 5: Classificacao de Issues e Root Cause Analysis
**Acao**: Categorizar issues identificadas e determinar causas-raiz
**Processo**:
1. **Issue Classification**:
   - **Critical**: Servico indisponivel, perda de dados, falhas de seguranca
   - **High**: Degradacao de performance, falhas intermitentes, alta taxa de erro
   - **Medium**: Warnings, configuracao subotima, problemas menores de performance
   - **Low**: Alertas informativos, oportunidades de otimizacao

2. **Root Cause Analysis**:
   - **Configuration Issues**: Configuracoes incorretas, dependencias ausentes
   - **Resource Constraints**: Limites de CPU/memoria/disco, throttling
   - **Network Issues**: Problemas de conectividade, resolucao DNS, regras de firewall
   - **Application Issues**: Bugs de codigo, memory leaks, queries ineficientes
   - **External Dependencies**: Falhas de servicos de terceiros, limites de API
   - **Security Issues**: Falhas de autenticacao, expiracao de certificados

3. **Impact Assessment**:
   - Determine impacto de negocio e usuarios/sistemas afetados
   - Avalie integridade de dados e implicacoes de seguranca
   - Avalie objetivos de tempo de recuperacao e prioridades

### Etapa 6: Gerar Plano de Remediacao
**Acao**: Criar um plano abrangente para tratar os problemas identificados
**Processo**:
1. **Immediate Actions** (issues Critical):
   - Correcoes emergenciais para restaurar disponibilidade
   - Workarounds temporarios para mitigar impacto
   - Procedimentos de escalacao para issues complexas

2. **Short-term Fixes** (issues High/Medium):
   - Ajustes de configuracao e scaling de recursos
   - Updates de aplicacao e patches
   - Melhorias de monitoramento e alertas

3. **Long-term Improvements** (todas as issues):
   - Mudancas arquiteturais para maior resiliencia
   - Medidas preventivas e melhorias de monitoramento
   - Melhorias de documentacao e processos

4. **Implementation Steps**:
   - Itens de acao priorizados com comandos Azure CLI especificos
   - Procedimentos de teste e validacao
   - Planos de rollback para cada mudanca
   - Monitoramento para verificar resolucao das issues

### Etapa 7: Confirmacao do Usuario e Geracao de Relatorio
**Acao**: Apresentar achados e obter aprovacao para acoes de remediacao
**Processo**:
1. **Exibir Resumo de Avaliacao de Saude**:
   ```
   🏥 Azure Resource Health Assessment

   📊 Resource Overview:
   • Resource: [Name] ([Type])
   • Status: [Healthy/Warning/Critical]
   • Location: [Region]
   • Last Analyzed: [Timestamp]

   🚨 Issues Identified:
   • Critical: X issues requiring immediate attention
   • High: Y issues affecting performance/reliability
   • Medium: Z issues for optimization
   • Low: N informational items

   🔍 Top Issues:
   1. [Issue Type]: [Description] - Impact: [High/Medium/Low]
   2. [Issue Type]: [Description] - Impact: [High/Medium/Low]
   3. [Issue Type]: [Description] - Impact: [High/Medium/Low]

   🛠️ Remediation Plan:
   • Immediate Actions: X items
   • Short-term Fixes: Y items
   • Long-term Improvements: Z items
   • Estimated Resolution Time: [Timeline]

   ❓ Proceed with detailed remediation plan? (y/n)
   ```

2. **Gerar Relatorio Detalhado**:
   ```markdown
   # Azure Resource Health Report: [Resource Name]

   **Generated**: [Timestamp]
   **Resource**: [Full Resource ID]
   **Overall Health**: [Status with color indicator]

   ## 🔍 Executive Summary
   [Brief overview of health status and key findings]

   ## 📊 Health Metrics
   - **Availability**: X% over last 24h
   - **Performance**: [Average response time/throughput]
   - **Error Rate**: X% over last 24h
   - **Resource Utilization**: [CPU/Memory/Storage percentages]

   ## 🚨 Issues Identified

   ### Critical Issues
   - **[Issue 1]**: [Description]
     - **Root Cause**: [Analysis]
     - **Impact**: [Business impact]
     - **Immediate Action**: [Required steps]

   ### High Priority Issues
   - **[Issue 2]**: [Description]
     - **Root Cause**: [Analysis]
     - **Impact**: [Performance/reliability impact]
     - **Recommended Fix**: [Solution steps]

   ## 🛠️ Remediation Plan

   ### Phase 1: Immediate Actions (0-2 hours)
   ```bash
   # Critical fixes to restore service
   [Azure CLI commands with explanations]
   ```

   ### Phase 2: Short-term Fixes (2-24 hours)
   ```bash
   # Performance and reliability improvements
   [Azure CLI commands with explanations]
   ```

   ### Phase 3: Long-term Improvements (1-4 weeks)
   ```bash
   # Architectural and preventive measures
   [Azure CLI commands and configuration changes]
   ```

   ## 📈 Monitoring Recommendations
   - **Alerts to Configure**: [List of recommended alerts]
   - **Dashboards to Create**: [Monitoring dashboard suggestions]
   - **Regular Health Checks**: [Recommended frequency and scope]

   ## ✅ Validation Steps
   - [ ] Verify issue resolution through logs
   - [ ] Confirm performance improvements
   - [ ] Test application functionality
   - [ ] Update monitoring and alerting
   - [ ] Document lessons learned

   ## 📝 Prevention Measures
   - [Recommendations to prevent similar issues]
   - [Process improvements]
   - [Monitoring enhancements]
   ```

## Error Handling
- **Resource Not Found**: Provide guidance on resource name/location specification
- **Authentication Issues**: Guide user through Azure authentication setup
- **Insufficient Permissions**: List required RBAC roles for resource access
- **No Logs Available**: Suggest enabling diagnostic settings and waiting for data
- **Query Timeouts**: Break down analysis into smaller time windows
- **Service-Specific Issues**: Provide generic health assessment with limitations noted

## Success Criteria
- ✅ Resource health status accurately assessed
- ✅ All significant issues identified and categorized
- ✅ Root cause analysis completed for major problems
- ✅ Actionable remediation plan with specific steps provided
- ✅ Monitoring and prevention recommendations included
- ✅ Clear prioritization of issues by business impact
- ✅ Implementation steps include validation and rollback procedures
