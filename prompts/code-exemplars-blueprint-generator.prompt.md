---
description: 'Gerador de prompt agnostico de tecnologia que cria prompts de IA customizaveis para varrer codebases e identificar code exemplars de alta qualidade. Suporta varias linguagens (.NET, Java, JavaScript, TypeScript, React, Angular, Python) com profundidade de analise configuravel, metodos de categorizacao e formatos de documentacao para estabelecer padroes de codigo e manter consistencia entre times de desenvolvimento.'
agent: 'agent'
---

# Gerador de Blueprint de Code Exemplars

## Variaveis de Configuracao
${PROJECT_TYPE="Auto-detect|.NET|Java|JavaScript|TypeScript|React|Angular|Python|Other"} <!-- Tecnologia principal -->
${SCAN_DEPTH="Basic|Standard|Comprehensive"} <!-- Profundidade de analise do codebase -->
${INCLUDE_CODE_SNIPPETS=true|false} <!-- Incluir code snippets reais alem de referencias de arquivo -->
${CATEGORIZATION="Pattern Type|Architecture Layer|File Type"} <!-- Como organizar os exemplars -->
${MAX_EXAMPLES_PER_CATEGORY=3} <!-- Numero maximo de exemplos por categoria -->
${INCLUDE_COMMENTS=true|false} <!-- Incluir comentarios explicativos para cada exemplar -->

## Prompt Gerado

"Faça o scan deste codebase e gere um arquivo exemplars.md que identifique exemplos de codigo representativos e de alta qualidade. Os exemplars devem demonstrar nossos padroes e patterns de codigo para ajudar a manter consistencia. Use a abordagem a seguir:

### 1. Fase de Analise do Codebase
- ${PROJECT_TYPE == "Auto-detect" ? "Detecte automaticamente linguagens e frameworks principais analisando extensoes de arquivo e arquivos de configuracao" : `Foque em arquivos de codigo ${PROJECT_TYPE}`}
- Identifique arquivos com implementacao de alta qualidade, boa documentacao e estrutura clara
- Procure patterns comumente usados, componentes arquiteturais e implementacoes bem estruturadas
- Priorize arquivos que demonstrem melhores praticas para nossa tecnologia
- Referencie apenas arquivos reais existentes no codebase - nada hipotetico

### 2. Criterios de Identificacao de Exemplars
- Codigo bem estruturado e legivel com convencoes de nome claras
- Comentarios e documentacao abrangentes
- Tratamento adequado de erros e validacao
- Aderencia a design patterns e principios arquiteturais
- Separacao de concerns e principio de responsabilidade unica
- Implementacao eficiente sem code smells
- Representativo das nossas abordagens padrao

### 3. Categorias de Padrões Core

${PROJECT_TYPE == ".NET" || PROJECT_TYPE == "Auto-detect" ? `#### .NET Exemplars (if detected)
- **Domain Models**: Entidades que implementam encapsulamento e logica de dominio
- **Repository Implementations**: Exemplos da nossa abordagem de data access
- **Service Layer Components**: Implementacoes bem estruturadas de business logic
- **Controller Patterns**: Controllers de API limpos com validacao e responses adequadas
- **Dependency Injection Usage**: Bons exemplos de configuracao e uso de DI
- **Middleware Components**: Implementacoes de middleware custom
- **Unit Test Patterns**: Testes bem estruturados com arranjo e asserts adequados` : ""}

${(PROJECT_TYPE == "JavaScript" || PROJECT_TYPE == "TypeScript" || PROJECT_TYPE == "React" || PROJECT_TYPE == "Angular" || PROJECT_TYPE == "Auto-detect") ? `#### Frontend Exemplars (if detected)
- **Component Structure**: Componentes limpos e bem estruturados
- **State Management**: Bons exemplos de state handling
- **API Integration**: Service calls e data handling bem implementados
- **Form Handling**: Padroes de validacao e submit
- **Routing Implementation**: Navegacao e configuracao de rotas
- **UI Components**: Componentes de UI reutilizaveis e bem estruturados
- **Unit Test Examples**: Testes de componente e service` : ""}

${PROJECT_TYPE == "Java" || PROJECT_TYPE == "Auto-detect" ? `#### Java Exemplars (if detected)
- **Entity Classes**: JPA entities ou domain models bem desenhados
- **Service Implementations**: Componentes de service layer limpos
- **Repository Patterns**: Implementacoes de data access
- **Controller/Resource Classes**: Implementacoes de endpoints de API
- **Configuration Classes**: Configuracao da aplicacao
- **Unit Tests**: Testes JUnit bem estruturados` : ""}

${PROJECT_TYPE == "Python" || PROJECT_TYPE == "Auto-detect" ? `#### Python Exemplars (if detected)
- **Class Definitions**: Classes bem estruturadas com documentacao adequada
- **API Routes/Views**: Implementacoes de API limpas
- **Data Models**: Definicoes de modelos ORM
- **Service Functions**: Implementacoes de business logic
- **Utility Modules**: Funcoes helper e utilitarios
- **Test Cases**: Testes unitarios bem estruturados` : ""}

### 4. Exemplars por Camada de Arquitetura

- **Presentation Layer**:
  - Componentes de UI
  - Controllers/API endpoints
  - View models/DTOs
  
- **Business Logic Layer**:
  - Implementacoes de services
  - Componentes de business logic
  - Orquestracao de workflows
  
- **Data Access Layer**:
  - Implementacoes de repositories
  - Data models
  - Query patterns
  
- **Cross-Cutting Concerns**:
  - Implementacoes de logging
  - Tratamento de erro
  - Authentication/authorization
  - Validation

### 5. Formato de Documentacao dos Exemplars

Para cada exemplar identificado, documente:
- Caminho do arquivo (relativo a raiz do repositorio)
- Breve descricao do que torna o arquivo exemplar
- Pattern ou tipo de componente que representa
${INCLUDE_COMMENTS ? "- Principais detalhes de implementacao e principios de codigo demonstrados" : ""}
${INCLUDE_CODE_SNIPPETS ? "- Code snippet pequeno e representativo (se aplicavel)" : ""}

${SCAN_DEPTH == "Comprehensive" ? `### 6. Documentacao Adicional

- **Consistency Patterns**: Observe padroes consistentes no codebase
- **Architecture Observations**: Documente patterns arquiteturais evidentes
- **Implementation Conventions**: Identifique convencoes de nomes e estrutura
- **Anti-patterns to Avoid**: Observe areas onde o codebase foge de best practices` : ""}

### ${SCAN_DEPTH == "Comprehensive" ? "7" : "6"}. Formato de Saida

Crie exemplars.md com:
1. Introducao explicando o proposito do documento
2. Table of contents com links para categorias
3. Secoes organizadas com base em ${CATEGORIZATION}
4. Ate ${MAX_EXAMPLES_PER_CATEGORY} exemplars por categoria
5. Conclusao com recomendacoes para manter qualidade de codigo

O documento deve ser acionavel para devs que precisam de orientacao ao implementar novas features consistentes com patterns existentes.

Importante: inclua apenas arquivos reais do codebase. Verifique se todos os caminhos de arquivo existem. Nao inclua exemplos placeholder ou hipoteticos.
"

## Expected Output
Ao executar este prompt, o GitHub Copilot varrera seu codebase e gerara um arquivo exemplars.md contendo referencias reais a exemplos de codigo de alta qualidade no repositorio, organizados conforme os parametros selecionados.
