---
description: 'Gerador abrangente de blueprint de arquitetura de projeto que analisa o código para criar documentação arquitetural detalhada. Detecta automaticamente stacks de tecnologia e padrões arquiteturais, gera diagramas visuais, documenta padrões de implementação e fornece blueprints extensíveis para manter a consistência arquitetural e orientar novos desenvolvimentos.'
agent: 'agent'
---

## Gerador Abrangente de Blueprint de Arquitetura de Projeto

## Variáveis de Configuração
${PROJECT_TYPE="Auto-detect|.NET|Java|React|Angular|Python|Node.js|Flutter|Other"} <!-- Tecnologia principal -->
${ARCHITECTURE_PATTERN="Auto-detect|Clean Architecture|Microservices|Layered|MVVM|MVC|Hexagonal|Event-Driven|Serverless|Monolithic|Other"} <!-- Padrão arquitetural principal -->
${DIAGRAM_TYPE="C4|UML|Flow|Component|None"} <!-- Tipo de diagrama de arquitetura -->
${DETAIL_LEVEL="High-level|Detailed|Comprehensive|Implementation-Ready"} <!-- Nível de detalhe a incluir -->
${INCLUDES_CODE_EXAMPLES=true|false} <!-- Incluir exemplos de código para ilustrar padrões -->
${INCLUDES_IMPLEMENTATION_PATTERNS=true|false} <!-- Incluir padrões de implementação detalhados -->
${INCLUDES_DECISION_RECORDS=true|false} <!-- Incluir registros de decisão arquitetural -->
${FOCUS_ON_EXTENSIBILITY=true|false} <!-- Enfatizar pontos e padrões de extensão -->

## Prompt Gerado

"Crie um documento abrangente 'Project_Architecture_Blueprint.md' que analise profundamente os padrões arquiteturais do código para servir como referência definitiva na manutenção da consistência arquitetural. Use a abordagem a seguir:

### 1. Detecção e Análise de Arquitetura
- ${PROJECT_TYPE == "Auto-detect" ? "Analise a estrutura do projeto para identificar todos os stacks de tecnologia e frameworks em uso examinando:
  - Arquivos de projeto e configuração
  - Dependências de pacotes e imports
  - Padrões e convenções específicas de framework
  - Configurações de build e deploy" : "Foque em padrões e práticas específicas de ${PROJECT_TYPE}"}

- ${ARCHITECTURE_PATTERN == "Auto-detect" ? "Determine o(s) padrão(ões) arquiteturais analisando:
  - Organização de pastas e namespacing
  - Dependency flow and component boundaries
  - Interface segregation and abstraction patterns
  - Communication mechanisms between components" : "Documente como a arquitetura ${ARCHITECTURE_PATTERN} é implementada"}

### 2. Visão Geral da Arquitetura
- Forneça uma explicação clara e concisa da abordagem arquitetural geral
- Documente os princípios orientadores evidentes nas escolhas arquiteturais
- Identifique limites arquiteturais e como são aplicados
- Observe quaisquer padrões híbridos ou adaptações de padrões

### 3. Visualização da Arquitetura
${DIAGRAM_TYPE != "None" ? `Crie diagramas ${DIAGRAM_TYPE} em múltiplos níveis de abstração:
- Visão geral de alto nível mostrando os principais subsistemas
- Diagramas de interação de componentes mostrando relacionamentos e dependências
- Diagramas de fluxo de dados mostrando como a informação percorre o sistema
- Garanta que os diagramas reflitam a implementação real, não padrões teóricos` : "Descreva os relacionamentos entre componentes com base nas dependências reais do código, fornecendo explicações textuais claras de:
- Organização e limites dos subsistemas
- Direções de dependência e interações entre componentes
- Fluxo de dados e sequências de processo"}

### 4. Componentes Arquiteturais Centrais
Para cada componente arquitetural descoberto no codebase:

- **Propósito e Responsabilidade**:
  - Função principal dentro da arquitetura
  - Domínios de negócio ou preocupações técnicas atendidas
  - Limites e escopo

- **Estrutura Interna**:
  - Organização de classes/módulos dentro do componente
  - Principais abstrações e suas implementações
  - Padrões de design utilizados

- **Padrões de Interação**:
  - Como o componente se comunica com outros
  - Interfaces expostas e consumidas
  - Padrões de dependency injection
  - Mecanismos de publicação/assinatura de eventos

- **Padrões de Evolução**:
  - Como o componente pode ser estendido
  - Pontos de variação e mecanismos de plugin
  - Abordagens de configuração e customização

### 5. Camadas Arquiteturais e Dependências
- Mapeie a estrutura de camadas conforme implementada no codebase
- Documente regras de dependência entre camadas
- Identifique mecanismos de abstração que permitem separação de camadas
- Observe dependências circulares ou violações de camadas
- Documente padrões de dependency injection usados para manter a separação

### 6. Arquitetura de Dados
- Documente a estrutura e organização do domain model
- Mapeie relacionamentos de entidades e padrões de agregação
- Identifique padrões de acesso a dados (repositories, data mappers, etc.)
- Documente abordagens de transformação e mapeamento de dados
- Observe estratégias e implementações de caching
- Documente padrões de validação de dados

### 7. Implementação de Cross-Cutting Concerns
Documente padrões de implementação para cross-cutting concerns:

- **Authentication & Authorization**:
  - Implementação do modelo de segurança
  - Padrões de enforcement de permissões
  - Abordagem de gestão de identidade
  - Padrões de limites de segurança

- **Error Handling & Resilience**:
  - Padrões de tratamento de exceções
  - Implementações de retry e circuit breaker
  - Estratégias de fallback e graceful degradation
  - Abordagens de reporte de erro e monitoring

- **Logging & Monitoring**:
  - Padrões de instrumentação
  - Implementação de observability
  - Fluxo de informações diagnósticas
  - Abordagem de performance monitoring

- **Validation**:
  - Estratégias de validação de entrada
  - Implementação de validação de regras de negócio
  - Distribuição de responsabilidades de validação
  - Padrões de reporte de erro

- **Configuration Management**:
  - Padrões de fontes de configuração
  - Estratégias de configuração por ambiente
  - Abordagem de gestão de secrets
  - Implementação de feature flags

### 8. Padrões de Comunicação entre Serviços
- Documente definições de limites de serviço
- Identifique protocolos e formatos de comunicação
- Mapeie padrões de comunicação síncrona vs. assíncrona
- Documente estratégias de versionamento de API
- Identifique mecanismos de service discovery
- Observe padrões de resiliência na comunicação entre serviços

### 9. Padrões Arquiteturais Específicos por Tecnologia
${PROJECT_TYPE == "Auto-detect" ? "Para cada stack de tecnologia detectado, documente padrões arquiteturais específicos:" : `Documente padrões arquiteturais específicos de ${PROJECT_TYPE}:`}

${(PROJECT_TYPE == ".NET" || PROJECT_TYPE == "Auto-detect") ?
"#### .NET Architectural Patterns (if detected)
- Host e application model
- Organização do pipeline de middleware
- Padrões de integração de serviços do framework
- Abordagens de ORM e acesso a dados
- Padrões de implementação de API (controllers, minimal APIs, etc.)
- Configuração do container de dependency injection" : ""}

${(PROJECT_TYPE == "Java" || PROJECT_TYPE == "Auto-detect") ?
"#### Java Architectural Patterns (if detected)
- Container da aplicação e processo de bootstrap
- Uso de framework de dependency injection (Spring, CDI, etc.)
- Padrões de implementação de AOP
- Gestão de limites transacionais
- Configuração e padrões de uso de ORM
- Padrões de implementação de serviços" : ""}

${(PROJECT_TYPE == "React" || PROJECT_TYPE == "Auto-detect") ?
"#### React Architectural Patterns (if detected)
- Estratégias de composição e reutilização de componentes
- Arquitetura de state management
- Padrões de tratamento de side effects
- Abordagem de routing e navigation
- Padrões de data fetching e caching
- Estratégias de otimização de rendering" : ""}

${(PROJECT_TYPE == "Angular" || PROJECT_TYPE == "Auto-detect") ?
"#### Angular Architectural Patterns (if detected)
- Estratégia de organização de módulos
- Design de hierarquia de componentes
- Padrões de services e dependency injection
- Abordagem de state management
- Padrões de programação reativa
- Implementação de route guards" : ""}

${(PROJECT_TYPE == "Python" || PROJECT_TYPE == "Auto-detect") ?
"#### Python Architectural Patterns (if detected)
- Abordagem de organização de módulos
- Estratégia de gerenciamento de dependências
- Padrões de implementação OOP vs. funcional
- Padrões de integração com framework
- Abordagem de programação assíncrona" : ""}

### 10. Padrões de Implementação
${INCLUDES_IMPLEMENTATION_PATTERNS ?
"Documente padrões concretos de implementação para componentes arquiteturais-chave:

- **Interface Design Patterns**:
  - Abordagens de interface segregation
  - Decisões de nível de abstração
  - Padrões de interface genérica vs. específica
  - Padrões de implementação padrão

- **Service Implementation Patterns**:
  - Gestão de lifecycle de serviços
  - Padrões de composição de serviços
  - Templates de implementação de operações
  - Tratamento de erros dentro de serviços

- **Repository Implementation Patterns**:
  - Implementações de query patterns
  - Gestão transacional
  - Tratamento de concorrência
  - Padrões de operações em lote

- **Controller/API Implementation Patterns**:
  - Padrões de tratamento de request
  - Abordagens de formatação de response
  - Validação de parâmetros
  - Implementação de versionamento de API

- **Domain Model Implementation**:
  - Padrões de implementação de entidades
  - Padrões de value objects
  - Implementação de domain events
  - Enforcement de regras de negócio" : "Mencione que padrões detalhados de implementação variam ao longo do codebase."}

### 11. Arquitetura de Testes
- Documente estratégias de teste alinhadas à arquitetura
- Identifique padrões de limite de teste (unit, integration, system)
- Mapeie test doubles e abordagens de mocking
- Documente estratégias de dados de teste
- Observe integração de ferramentas e frameworks de teste

### 12. Arquitetura de Deploy
- Documente a topologia de deploy derivada da configuração
- Identifique adaptações arquiteturais específicas por ambiente
- Mapeie padrões de resolução de dependências em runtime
- Documente gestão de configuração entre ambientes
- Identifique abordagens de containerization e orchestration
- Observe padrões de integração com serviços de cloud

### 13. Padrões de Extensão e Evolução
${FOCUS_ON_EXTENSIBILITY ?
"Forneça orientação detalhada para estender a arquitetura:

- **Feature Addition Patterns**:
  - Como adicionar novas features preservando a integridade arquitetural
  - Onde posicionar novos componentes por tipo
  - Diretrizes para introdução de dependências
  - Padrões de extensão de configuração

- **Modification Patterns**:
  - Como modificar com segurança componentes existentes
  - Estratégias para manter backward compatibility
  - Padrões de depreciação
  - Abordagens de migração

- **Integration Patterns**:
  - Como integrar novos sistemas externos
  - Padrões de implementação de adapters
  - Padrões de anti-corruption layer
  - Implementação de service facade" : "Documente pontos-chave de extensão na arquitetura."}

${INCLUDES_CODE_EXAMPLES ?
"### 14. Exemplos de Padrões Arquiteturais
Extraia exemplos representativos de código que ilustrem padrões arquiteturais-chave:

- **Layer Separation Examples**:
  - Separação entre definição e implementação de interface
  - Padrões de comunicação entre camadas
  - Exemplos de dependency injection

- **Component Communication Examples**:
  - Padrões de invocação de serviços
  - Publicação e tratamento de eventos
  - Implementação de passagem de mensagens

- **Extension Point Examples**:
  - Registro e descoberta de plugins
  - Implementações de interfaces de extensão
  - Padrões de extensão guiados por configuração

Inclua contexto suficiente em cada exemplo para mostrar o padrão claramente, mas mantenha os exemplos concisos e focados em conceitos arquiteturais." : ""}

${INCLUDES_DECISION_RECORDS ?
"### 15. Architectural Decision Records
Documente decisões arquiteturais-chave evidentes no codebase:

- **Architectural Style Decisions**:
  - Por que o padrão arquitetural atual foi escolhido
  - Alternativas consideradas (com base na evolução do código)
  - Restrições que influenciaram a decisão

- **Technology Selection Decisions**:
  - Escolhas de tecnologia e impacto arquitetural
  - Racionais de seleção de framework
  - Decisões de componente custom vs. off-the-shelf

- **Implementation Approach Decisions**:
  - Padrões de implementação específicos escolhidos
  - Adaptações de padrões
  - Tradeoffs de performance vs. maintainability

Para cada decisão, observe:
- Contexto que tornou a decisão necessária
- Fatores considerados na decisão
- Consequências resultantes (positivas e negativas)
- Flexibilidade futura ou limitações introduzidas" : ""}

### ${INCLUDES_DECISION_RECORDS ? "16" : INCLUDES_CODE_EXAMPLES ? "15" : "14"}. Governança da Arquitetura
- Documente como a consistência arquitetural é mantida
- Identifique verificações automatizadas de conformidade arquitetural
- Observe processos de revisão arquitetural evidentes no codebase
- Documente práticas de documentação arquitetural

### ${INCLUDES_DECISION_RECORDS ? "17" : INCLUDES_CODE_EXAMPLES ? "16" : "15"}. Blueprint para Novo Desenvolvimento
Crie um guia arquitetural claro para implementar novas features:

- **Development Workflow**:
  - Pontos de partida para diferentes tipos de feature
  - Sequência de criação de componentes
  - Passos de integração com a arquitetura existente
  - Abordagem de testes por camada arquitetural

- **Implementation Templates**:
  - Templates de classe/base interface para componentes arquiteturais-chave
  - Organização padrão de arquivos para novos componentes
  - Padrões de declaração de dependências
  - Requisitos de documentação

- **Common Pitfalls**:
  - Violações arquiteturais a evitar
  - Erros arquiteturais comuns
  - Considerações de performance
  - Pontos cegos de teste

Inclua informações sobre quando este blueprint foi gerado e recomendações para mantê-lo atualizado conforme a arquitetura evolui."
