---
agent: 'agent'
description: 'Gere planos de rollout abrangentes com preflight checks, deployment passo a passo, sinais de verificacao, rollback e comunicacao para mudancas de infraestrutura e aplicacao'
tools: ['codebase', 'terminalCommand', 'search', 'githubRepo']
---

# Gerador de Plano de Rollout DevOps

Seu objetivo e criar um plano de rollout abrangente e pronto para producao para mudancas de infraestrutura ou aplicacao.

## Requisitos de Entrada

Colete estes detalhes antes de gerar o plano:

### Descricao da Mudanca
- O que esta mudando (infraestrutura, aplicacao, configuracao)
- Transicao de versao ou estado (from/to)
- Problema resolvido ou feature adicionada

### Detalhes do Ambiente
- Ambiente alvo (dev, staging, producao, todos)
- Tipo de infraestrutura (Kubernetes, VMs, serverless, containers)
- Servicos e dependencias afetados
- Capacidade e escala atuais

### Restricoes e Requisitos
- Janela de downtime aceitavel
- Restricoes de change window
- Requisitos de aprovacao
- Consideracoes regulatórias ou compliance

### Avaliacao de Risco
- Blast radius da mudanca
- Data migrations ou schema changes
- Complexidade e seguranca de rollback
- Riscos conhecidos

## Formato de Saida

Gere um plano estruturado com estas secoes:

### 1. Executive Summary
- O que, por que, quando, duracao
- Nivel de risco e tempo de rollback
- Sistemas afetados e impacto ao usuario
- Downtime esperado

### 2. Prerequisitos e Aprovacoes
- Aprovacoes necessarias (technical lead, seguranca, compliance, negocio)
- Recursos necessarios (capacidade, backups, monitoramento, automacao de rollback)
- Backups pre-deployment

### 3. Preflight Checks
- Validacao de saude da infraestrutura
- Baseline de saude da aplicacao
- Disponibilidade de dependencias
- Baseline de metricas de monitoramento
- Checklist go/no-go

### 4. Procedimento de Rollout Passo a Passo
**Fases**: Pre-deployment, deployment, verificacao progressiva
- Comandos especificos para cada etapa
- Validacao apos cada etapa
- Estimativas de duracao

### 5. Sinais de Verificacao
**Immediate** (0-2 min): Deployment ok, pods/containers ativos, health checks passando
**Short-term** (2-5 min): Aplicacao respondendo, error rates aceitaveis, latencia normal
**Medium-term** (5-15 min): Metricas sustentadas, conexoes estaveis, integracoes ok
**Long-term** (15+ min): Sem degradacao, capacidade saudavel, metricas de negocio normais

### 6. Procedimento de Rollback
**Decision Criteria**: Quando iniciar rollback
**Rollback Steps**: Automacao, revert de infra ou restore completo
**Post-Rollback Verification**: Confirmar saude restaurada
**Communication**: Notificacao de stakeholders

### 7. Plano de Comunicacao
- Pre-deployment (T-24h): Aviso de agenda e impacto
- Inicio do deployment: Aviso de inicio
- Atualizacoes de progresso: Status a cada X minutos
- Conclusao: Confirmacao de sucesso
- Rollback (se necessario): Notificacao de incidente

**Stakeholder Matrix**: Quem notificar, quando, por qual canal e com que conteudo

### 8. Tarefas Pos-Deployment
- Immediate (1h): Verificar criterios, revisar logs
- Short-term (24h): Monitorar metricas, revisar erros
- Medium-term (1 week): Post-deployment review, lessons learned

### 9. Planos de Contingencia
Cenarios: Falha parcial, degradacao de performance, inconsistencia de dados, falha de dependencia
Para cada: Sintomas, resposta, timeline

### 10. Informacoes de Contato
- On-call primario e secundario
- Caminho de escalacao
- Contatos de emergencia (infra, seguranca, banco, rede)

## Customizacao do Plano

Adapte conforme:
- **Tipo de Infraestrutura**: Kubernetes, VMs, serverless, databases
- **Nivel de Risco**: Low (simplificado), medium (padrao), high (gates adicionais)
- **Tipo de Mudanca**: Code deployment, infraestrutura, configuracao, data migration
- **Ambiente**: Producao (plano completo), staging (simplificado), development (minimo)

## Lembretes

- Sempre tenha plano de rollback testado
- Comunique cedo e com frequencia
- Monitore metricas, nao apenas logs
- Documente tudo
- Aprenda com cada deployment
- Nunca deploy na sexta a tarde (a menos que seja critico)
- Nunca pule etapas de verificacao
- Nunca assuma que "vai funcionar"
