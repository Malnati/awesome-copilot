---
agent: 'agent'
description: 'Prompt para criar a arquitetura técnica de alto nível de um Epic, baseada em um Product Requirements Document.'
---

## Prompt de Especificação de Arquitetura de Epic

## Objetivo

Atue como um Arquiteto de Software Sênior. Sua tarefa é pegar um Epic PRD e criar uma especificação de arquitetura técnica de alto nível. Este documento irá guiar o desenvolvimento do epic, detalhando os principais componentes, funcionalidades e habilitadores técnicos necessários.

## Considerações de Contexto

- O Epic PRD do Product Manager.
- Padrão **Domain-driven architecture** para aplicações modulares e escaláveis.
- Requisitos de deployment **Self-hosted e SaaS**.
- **Docker containerization** para todos os serviços.
- Stack **TypeScript/Next.js** com App Router.
- Padrões de monorepo **Turborepo**.
- **tRPC** para APIs type-safe.
- **Stack Auth** para autenticação.

**Nota:** NÃO escreva código na saída, exceto pseudocódigo para situações técnicas.

## Formato de Saída

A saída deve ser uma Especificação de Arquitetura de Epic completa em formato Markdown, salva em `/docs/ways-of-work/plan/{epic-name}/arch.md`.

### Estrutura da Especificação

#### 1. Visão Geral da Arquitetura do Epic

- Um breve resumo da abordagem técnica para o epic.

#### 2. Diagrama de Arquitetura do Sistema

Crie um diagrama Mermaid abrangente que ilustre toda a arquitetura do sistema para este epic. O diagrama deve incluir:

- **Camada de Usuário**: Mostre como diferentes tipos de usuários (navegadores web, apps móveis, interfaces admin) interagem com o sistema
- **Camada de Aplicação**: Represente load balancers, instâncias de aplicação e serviços de autenticação (Stack Auth)
- **Camada de Serviço**: Inclua APIs tRPC, serviços de background, engines de workflow (n8n) e quaisquer serviços específicos do epic
- **Camada de Dados**: Mostre bancos de dados (PostgreSQL), bancos vetoriais (Qdrant), camadas de cache (Redis) e integrações com APIs externas
- **Camada de Infraestrutura**: Represente a containerização Docker e arquitetura de deploy

Use subgraphs claros para organizar essas camadas, aplique codificação de cores consistente para diferentes tipos de componentes e mostre o fluxo de dados entre componentes. Inclua caminhos de requisição síncronos e fluxos de processamento assíncronos quando relevante para o epic.

#### 3. Funcionalidades de Alto Nível & Habilitadores Técnicos

- Uma lista das funcionalidades de alto nível a serem construídas.
- Uma lista dos habilitadores técnicos (ex: novos serviços, bibliotecas, infraestrutura) necessários para suportar as funcionalidades.

#### 4. Stack Tecnológico

- Uma lista das principais tecnologias, frameworks e bibliotecas a serem usadas.

#### 5. Valor Técnico

- Estime o valor técnico (ex: Alto, Médio, Baixo) com uma breve justificativa.

#### 6. Estimativa T-Shirt Size

- Forneça uma estimativa de t-shirt size de alto nível para o epic (ex: P, M, G, GG).

## Template de Contexto

- **Epic PRD:** [O conteúdo do arquivo markdown Epic PRD]
