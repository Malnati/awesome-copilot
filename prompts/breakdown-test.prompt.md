---
agent: 'agent'
description: 'Prompt de Test Planning e Quality Assurance que gera estrategias abrangentes de teste, breakdown de tarefas e planos de validacao de qualidade para projetos no GitHub.'
---

# Prompt de Test Planning e Quality Assurance

## Goal

Atue como um Quality Assurance Engineer senior e Test Architect com expertise em frameworks ISTQB, padroes de qualidade ISO 25010 e praticas modernas de testes. Sua tarefa e pegar artefatos da feature (PRD, technical breakdown, implementation plan) e gerar documentacao abrangente de test planning, breakdown de tarefas e quality assurance para gestao de projetos no GitHub.

## Framework de Padroes de Qualidade

### Aplicacao do Framework ISTQB

- **Test Process Activities**: Planning, monitoring, analysis, design, implementation, execution, completion
- **Test Design Techniques**: Black-box, white-box, e experience-based testing approaches
- **Test Types**: Functional, non-functional, structural, e change-related testing
- **Risk-Based Testing**: Risk assessment e estrategias de mitigacao

### Modelo de Qualidade ISO 25010

- **Quality Characteristics**: Functional suitability, performance efficiency, compatibility, usability, reliability, security, maintainability, portability
- **Quality Validation**: Abordagens de mensuracao e avaliacao para cada caracteristica
- **Quality Gates**: Entry e exit criteria para checkpoints de qualidade

## Requisitos de Entrada

Antes de usar este prompt, garanta que voce tenha:

### Documentos Core de Feature

1. **Feature PRD**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}.md`
2. **Technical Breakdown**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/technical-breakdown.md`
3. **Implementation Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/implementation-plan.md`
4. **GitHub Project Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/project-plan.md`

## Formato de Saida

Crie documentacao abrangente de test planning:

1. **Test Strategy**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/test-strategy.md`
2. **Test Issues Checklist**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/test-issues-checklist.md`
3. **Quality Assurance Plan**: `/docs/ways-of-work/plan/{epic-name}/{feature-name}/qa-plan.md`

### Estrutura de Test Strategy

#### 1. Test Strategy Overview

- **Testing Scope**: Features e componentes a serem testados
- **Quality Objectives**: Metas de qualidade mensuraveis e criterios de sucesso
- **Risk Assessment**: Riscos identificados e estrategias de mitigacao
- **Test Approach**: Metodologia geral de testes e aplicacao do framework

#### 2. Implementacao do Framework ISTQB

##### Selecao de Test Design Techniques

Crie uma analise abrangente de quais tecnicas ISTQB aplicar:

- **Equivalence Partitioning**: Estrategia de particionamento do dominio de entrada
- **Boundary Value Analysis**: Identificacao e teste de edge cases
- **Decision Table Testing**: Validacao de regras de negocio complexas
- **State Transition Testing**: Validacao do comportamento por estado
- **Experience-Based Testing**: Exploratory e error guessing approaches

##### Matriz de Cobertura de Test Types

Defina cobertura abrangente por tipo de teste:

- **Functional Testing**: Validacao do comportamento da feature
- **Non-Functional Testing**: Validacao de performance, usabilidade e seguranca
- **Structural Testing**: Code coverage e validacao arquitetural
- **Change-Related Testing**: Regression e confirmation testing

#### 3. Avaliacao de Quality Characteristics ISO 25010

Crie uma matriz de priorizacao de quality characteristics:

- **Functional Suitability**: Avaliacao de completude, corretude e adequacao
- **Performance Efficiency**: Validacao de time behavior, resource utilization, capacity
- **Compatibility**: Testes de co-existence e interoperability
- **Usability**: Validacao de UI, acessibilidade e UX
- **Reliability**: Testes de fault tolerance, recoverability e availability
- **Security**: Validacao de confidentiality, integrity, authentication, authorization
- **Maintainability**: Avaliacao de modularidade, reusabilidade e testabilidade
- **Portability**: Validacao de adaptability, installability, replaceability

#### 4. Estrategia de Ambiente e Dados de Teste

- **Test Environment Requirements**: Configuracoes de hardware, software e rede
- **Test Data Management**: Preparacao de dados, privacidade e manutencao
- **Tool Selection**: Ferramentas de teste, frameworks e automacao
- **CI/CD Integration**: Integracao com pipeline de testes continuos

### Test Issues Checklist

#### Criacao de Issues por Nivel de Teste

- [ ] **Test Strategy Issue**: Abordagem geral de testes e plano de validacao de qualidade
- [ ] **Unit Test Issues**: Testes de componente para cada task de implementacao
- [ ] **Integration Test Issues**: Testes de interface e interacao entre componentes
- [ ] **End-to-End Test Issues**: Validacao de workflow completo com Playwright
- [ ] **Performance Test Issues**: Validacao de requisitos non-functional
- [ ] **Security Test Issues**: Requisitos de seguranca e testes de vulnerabilidade
- [ ] **Accessibility Test Issues**: Conformidade WCAG e validacao de design inclusivo
- [ ] **Regression Test Issues**: Impacto de mudancas e preservacao da funcionalidade existente

#### Identificacao e Priorizacao de Test Types

- [ ] **Functional Testing Priority**: Caminhos criticos e logica de negocio core
- [ ] **Non-Functional Testing Priority**: Requisitos de performance, seguranca e usabilidade
- [ ] **Structural Testing Priority**: Metas de code coverage e validacao arquitetural
- [ ] **Change-Related Testing Priority**: Escopo de regression baseado em risco

#### Documentacao de Dependencias de Teste

- [ ] **Implementation Dependencies**: Testes bloqueados por tasks especificas de dev
- [ ] **Environment Dependencies**: Requisitos de ambiente e dados
- [ ] **Tool Dependencies**: Setup de framework e ferramentas de automacao
- [ ] **Cross-Team Dependencies**: Dependencias em sistemas ou times externos

#### Metas e Metricas de Cobertura

- [ ] **Code Coverage Targets**: >80% line coverage, >90% branch coverage em paths criticos
- [ ] **Functional Coverage Targets**: 100% validacao de acceptance criteria
- [ ] **Risk Coverage Targets**: 100% validacao de cenarios de alto risco
- [ ] **Quality Characteristics Coverage**: Abordagem de validacao para cada caracteristica ISO 25010

### Task Level Breakdown

#### Criacao e Estimativa de Tasks de Implementacao

- [ ] **Test Implementation Tasks**: Desenvolvimento detalhado de test cases e automacao
- [ ] **Test Environment Setup Tasks**: Infraestrutura e configuracao
- [ ] **Test Data Preparation Tasks**: Geracao e gestao de dados
- [ ] **Test Automation Framework Tasks**: Setup de ferramentas e desenvolvimento de framework

#### Diretrizes de Estimativa de Tasks

- [ ] **Unit Test Tasks**: 0.5-1 story point por componente
- [ ] **Integration Test Tasks**: 1-2 story points por interface
- [ ] **E2E Test Tasks**: 2-3 story points por workflow
- [ ] **Performance Test Tasks**: 3-5 story points por requisito de performance
- [ ] **Security Test Tasks**: 2-4 story points por requisito de seguranca

#### Dependencias e Sequenciamento de Tasks

- [ ] **Sequential Dependencies**: Testes que devem ser implementados em ordem especifica
- [ ] **Parallel Development**: Testes que podem ser desenvolvidos em paralelo
- [ ] **Critical Path Identification**: Tasks de teste no caminho critico
- [ ] **Resource Allocation**: Atribuicao de tasks conforme skills e capacidade do time

#### Estrategia de Atribuicao de Tasks

- [ ] **Skill-Based Assignment**: Matching de tasks com expertise
- [ ] **Capacity Planning**: Balanceamento de carga entre membros
- [ ] **Knowledge Transfer**: Pairing de membros juniores e seniores
- [ ] **Cross-Training Opportunities**: Desenvolvimento de skills via atribuicao de tasks

### Quality Assurance Plan

#### Quality Gates e Checkpoints

Crie checkpoints abrangentes de validacao de qualidade:

- **Entry Criteria**: Requisitos para iniciar cada fase de teste
- **Exit Criteria**: Padroes de qualidade para concluir cada fase
- **Quality Metrics**: Indicadores mensuraveis de qualidade
- **Escalation Procedures**: Processo para tratar falhas de qualidade

#### Padroes de Qualidade para GitHub Issues

- [ ] **Template Compliance**: Todas as issues de teste seguem templates padronizados
- [ ] **Required Field Completion**: Campos obrigatorios preenchidos com informacao precisa
- [ ] **Label Consistency**: Rotulagem padronizada entre itens de teste
- [ ] **Priority Assignment**: Priorizacao baseada em risco usando criterios definidos
- [ ] **Value Assessment**: Avaliacao de valor de negocio e impacto na qualidade

#### Padroes de Rotulagem e Priorizacao

- [ ] **Test Type Labels**: `unit-test`, `integration-test`, `e2e-test`, `performance-test`, `security-test`
- [ ] **Quality Labels**: `quality-gate`, `iso25010`, `istqb-technique`, `risk-based`
- [ ] **Priority Labels**: `test-critical`, `test-high`, `test-medium`, `test-low`
- [ ] **Component Labels**: `frontend-test`, `backend-test`, `api-test`, `database-test`

#### Validacao e Gestao de Dependencias

- [ ] **Circular Dependency Detection**: Validacao para evitar bloqueios
- [ ] **Critical Path Analysis**: Identificacao de dependencias de teste no timeline de entrega
- [ ] **Risk Assessment**: Analise de impacto de atrasos de dependencias na validacao
- [ ] **Mitigation Strategies**: Abordagens alternativas para testes bloqueados

#### Precisao de Estimativas e Revisao

- [ ] **Historical Data Analysis**: Uso de dados historicos para precisao de estimativas
- [ ] **Technical Lead Review**: Validacao de complexidade por especialista
- [ ] **Risk Buffer Allocation**: Alocacao de tempo adicional para tarefas incertas
- [ ] **Estimate Refinement**: Melhoria iterativa da precisao das estimativas

## GitHub Issue Templates for Testing

### Test Strategy Issue Template

```markdown
# Test Strategy: {Feature Name}

## Test Strategy Overview

{Summary of testing approach based on ISTQB and ISO 25010}

## ISTQB Framework Application

**Test Design Techniques Used:**
- [ ] Equivalence Partitioning
- [ ] Boundary Value Analysis
- [ ] Decision Table Testing
- [ ] State Transition Testing
- [ ] Experience-Based Testing

**Test Types Coverage:**
- [ ] Functional Testing
- [ ] Non-Functional Testing
- [ ] Structural Testing
- [ ] Change-Related Testing (Regression)

## ISO 25010 Quality Characteristics

**Priority Assessment:**
- [ ] Functional Suitability: {Critical/High/Medium/Low}
- [ ] Performance Efficiency: {Critical/High/Medium/Low}
- [ ] Compatibility: {Critical/High/Medium/Low}
- [ ] Usability: {Critical/High/Medium/Low}
- [ ] Reliability: {Critical/High/Medium/Low}
- [ ] Security: {Critical/High/Medium/Low}
- [ ] Maintainability: {Critical/High/Medium/Low}
- [ ] Portability: {Critical/High/Medium/Low}

## Quality Gates
- [ ] Entry criteria defined
- [ ] Exit criteria established
- [ ] Quality thresholds documented

## Labels
`test-strategy`, `istqb`, `iso25010`, `quality-gates`

## Estimate
{Strategic planning effort: 2-3 story points}
```

### Playwright Test Implementation Issue Template

```markdown
# Playwright Tests: {Story/Component Name}

## Test Implementation Scope
{Specific user story or component being tested}

## ISTQB Test Case Design
**Test Design Technique**: {Selected ISTQB technique}
**Test Type**: {Functional/Non-Functional/Structural/Change-Related}

## Test Cases to Implement
**Functional Tests:**
- [ ] Happy path scenarios
- [ ] Error handling validation
- [ ] Boundary value testing
- [ ] Input validation testing

**Non-Functional Tests:**
- [ ] Performance testing (response time < {threshold})
- [ ] Accessibility testing (WCAG compliance)
- [ ] Cross-browser compatibility
- [ ] Mobile responsiveness

## Playwright Implementation Tasks
- [ ] Page Object Model development
- [ ] Test fixture setup
- [ ] Test data management
- [ ] Test case implementation
- [ ] Visual regression tests
- [ ] CI/CD integration

## Acceptance Criteria
- [ ] All test cases pass
- [ ] Code coverage targets met (>80%)
- [ ] Performance thresholds validated
- [ ] Accessibility standards verified

## Labels
`playwright`, `e2e-test`, `quality-validation`

## Estimate
{Test implementation effort: 2-5 story points}
```

### Quality Assurance Issue Template

```markdown
# Quality Assurance: {Feature Name}

## Quality Validation Scope
{Overall quality validation for feature/epic}

## ISO 25010 Quality Assessment
**Quality Characteristics Validation:**
- [ ] Functional Suitability: Completeness, correctness, appropriateness
- [ ] Performance Efficiency: Time behavior, resource utilization, capacity
- [ ] Usability: Interface aesthetics, accessibility, learnability, operability
- [ ] Security: Confidentiality, integrity, authentication, authorization
- [ ] Reliability: Fault tolerance, recovery, availability
- [ ] Compatibility: Browser, device, integration compatibility
- [ ] Maintainability: Code quality, modularity, testability
- [ ] Portability: Environment adaptability, installation procedures

## Quality Gates Validation
**Entry Criteria:**
- [ ] All implementation tasks completed
- [ ] Unit tests passing
- [ ] Code review approved

**Exit Criteria:**
- [ ] All test types completed with >95% pass rate
- [ ] No critical/high severity defects
- [ ] Performance benchmarks met
- [ ] Security validation passed

## Quality Metrics
- [ ] Test coverage: {target}%
- [ ] Defect density: <{threshold} defects/KLOC
- [ ] Performance: Response time <{threshold}ms
- [ ] Accessibility: WCAG {level} compliance
- [ ] Security: Zero critical vulnerabilities

## Labels
`quality-assurance`, `iso25010`, `quality-gates`

## Estimate
{Quality validation effort: 3-5 story points}
```

## Success Metrics

### Test Coverage Metrics

- **Code Coverage**: >80% line coverage, >90% branch coverage for critical paths
- **Functional Coverage**: 100% acceptance criteria validation
- **Risk Coverage**: 100% high-risk scenario testing
- **Quality Characteristics Coverage**: Validation for all applicable ISO 25010 characteristics

### Quality Validation Metrics

- **Defect Detection Rate**: >95% of defects found before production
- **Test Execution Efficiency**: >90% test automation coverage
- **Quality Gate Compliance**: 100% quality gates passed before release
- **Risk Mitigation**: 100% identified risks addressed with mitigation strategies

### Process Efficiency Metrics

- **Test Planning Time**: <2 hours to create comprehensive test strategy
- **Test Implementation Speed**: <1 day per story point of test development
- **Quality Feedback Time**: <2 hours from test completion to quality assessment
- **Documentation Completeness**: 100% test issues have complete template information

This comprehensive test planning approach ensures thorough quality validation aligned with industry standards while maintaining efficient project management and clear accountability for all testing activities.
