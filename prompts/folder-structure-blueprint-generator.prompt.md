---
description: 'Prompt abrangente e agnostico de tecnologia para analisar e documentar estruturas de pastas de projetos. Auto-detecta tipos de projeto (.NET, Java, React, Angular, Python, Node.js, Flutter), gera blueprints detalhados com opcoes de visualizacao, convencoes de nome, patterns de alocacao de arquivos e templates de extensao para manter organizacao consistente em diferentes stacks.'
agent: 'agent'
---

# Gerador de Blueprint de Estrutura de Pastas de Projeto

## Variaveis de Configuracao

${PROJECT_TYPE="Auto-detect|.NET|Java|React|Angular|Python|Node.js|Flutter|Other"} 
<!-- Selecionar tecnologia principal -->

${INCLUDES_MICROSERVICES="Auto-detect|true|false"} 
<!-- Esta arquitetura usa microservices? -->

${INCLUDES_FRONTEND="Auto-detect|true|false"} 
<!-- O projeto inclui componentes de frontend? -->

${IS_MONOREPO="Auto-detect|true|false"} 
<!-- E um monorepo com multiplos projetos? -->

${VISUALIZATION_STYLE="ASCII|Markdown List|Table"} 
<!-- Como visualizar a estrutura -->

${DEPTH_LEVEL=1-5} 
<!-- Quantos niveis de pastas detalhar -->

${INCLUDE_FILE_COUNTS=true|false} 
<!-- Incluir estatisticas de contagem de arquivos -->

${INCLUDE_GENERATED_FOLDERS=true|false} 
<!-- Incluir pastas geradas automaticamente -->

${INCLUDE_FILE_PATTERNS=true|false} 
<!-- Documentar patterns de nome/localizacao de arquivos -->

${INCLUDE_TEMPLATES=true|false} 
<!-- Incluir templates de arquivos/pastas para novas features -->

## Prompt Gerado

"Analise a estrutura de pastas do projeto e crie um documento abrangente 'Project_Folders_Structure_Blueprint.md' que sirva como guia definitivo para manter organizacao consistente de codigo. Use a abordagem abaixo:

### Fase Inicial de Auto-deteccao

${PROJECT_TYPE == "Auto-detect" ? 
"Comece varrendo a estrutura de pastas em busca de arquivos-chave que identifiquem o tipo de projeto:
- Procure arquivos de solucao/projeto (.sln, .csproj, .fsproj, .vbproj) para identificar projetos .NET
- Verifique arquivos de build (pom.xml, build.gradle, settings.gradle) para projetos Java
- Identifique package.json com dependencias para projetos JavaScript/TypeScript
- Procure arquivos especificos de framework (angular.json, entradas react-scripts, next.config.js)
- Verifique identificadores de projeto Python (requirements.txt, setup.py, pyproject.toml)
- Examine identificadores de apps mobile (pubspec.yaml, pastas android/ios)
- Observe todas as assinaturas de tecnologia encontradas e suas versoes" : 
"Foque a analise na estrutura do projeto ${PROJECT_TYPE}"}

${IS_MONOREPO == "Auto-detect" ? 
"Determine se e monorepo procurando:
- Multiplos projetos distintos com seus proprios arquivos de configuracao
- Arquivos de configuracao de workspace (lerna.json, nx.json, turborepo.json, etc.)
- Referencias cruzadas entre projetos e patterns de dependencias compartilhadas
- Scripts e configuracoes de orquestracao na raiz" : ""}

${INCLUDES_MICROSERVICES == "Auto-detect" ? 
"Verifique indicadores de arquitetura de microservices:
- Multiplos diretorios de service com estruturas similares
- Dockerfiles ou configuracoes de deploy por service
- Patterns de comunicacao entre services (APIs, message brokers)
- Configuracoes de service registry ou discovery
- Arquivos de configuracao de API gateway
- Libraries ou utilities compartilhadas entre services" : ""}

${INCLUDES_FRONTEND == "Auto-detect" ? 
"Identifique componentes de frontend procurando:
- Diretorios de assets web (wwwroot, public, dist, static)
- Arquivos de UI framework (components, modules, pages)
- Configuracoes de build de frontend (webpack, vite, rollup, etc.)
- Organizacao de stylesheets (CSS, SCSS, styled-components)
- Organizacao de assets estaticos (images, fonts, icons)" : ""}

### 1. Structural Overview

Forneca uma visao geral de alto nivel dos principios de organizacao e estrutura do projeto ${PROJECT_TYPE == "Auto-detect" ? "detectado" : PROJECT_TYPE}:

- Documente a abordagem arquitetural refletida na estrutura
- Identifique principios organizacionais principais (por feature, por camada, por dominio, etc.)
- Observe patterns estruturais que se repetem no codebase
- Documente a razao por tras da estrutura quando possivel inferir

${IS_MONOREPO == "Auto-detect" ? 
"Se detectado como monorepo, explique como ele e organizado e a relacao entre projetos." : 
IS_MONOREPO ? "Explique como o monorepo e organizado e a relacao entre projetos." : ""}

${INCLUDES_MICROSERVICES == "Auto-detect" ? 
"Se microservices forem detectados, descreva como sao estruturados e organizados." : 
INCLUDES_MICROSERVICES ? "Descreva como os microservices sao estruturados e organizados." : ""}

### 2. Directory Visualization

${VISUALIZATION_STYLE == "ASCII" ? 
"Crie uma arvore ASCII da hierarquia de pastas ate o nivel ${DEPTH_LEVEL}." : ""}

${VISUALIZATION_STYLE == "Markdown List" ? 
"Use listas Markdown aninhadas para representar a hierarquia ate o nivel ${DEPTH_LEVEL}." : ""}

${VISUALIZATION_STYLE == "Table" ? 
"Crie uma tabela com colunas Path, Purpose, Content Types e Conventions." : ""}

${INCLUDE_GENERATED_FOLDERS ? 
"Inclua todas as pastas, inclusive as geradas." : 
"Exclua pastas geradas automaticamente como bin/, obj/, node_modules/, etc."}

### 3. Key Directory Analysis

Documente o proposito, conteudo e patterns de cada diretorio significativo:

${PROJECT_TYPE == "Auto-detect" ? 
"Para cada tecnologia detectada, analise a estrutura com base nos patterns observados:" : ""}

${(PROJECT_TYPE == ".NET" || PROJECT_TYPE == "Auto-detect") ? 
"#### .NET Project Structure (if detected)

- **Solution Organization**: 
  - Como projetos sao agrupados e relacionados
  - Patterns de organizacao de solution folders
  - Patterns de projetos multi-target

- **Project Organization**:
  - Patterns internos de estrutura de pastas
  - Abordagem de organizacao do source
  - Organizacao de resources
  - Dependencias e referencias de projeto

- **Domain/Feature Organization**:
  - Separacao de dominios ou features
  - Patterns de enforcement de limites de dominio

- **Layer Organization**:
  - Separacao de concerns (Controllers, Services, Repositories, etc.)
  - Patterns de interacao e dependencia entre camadas

- **Configuration Management**:
  - Localizacao e proposito de arquivos de configuracao
  - Configuracoes por ambiente
  - Abordagem de gestao de secrets

- **Test Project Organization**:
  - Estrutura e convencoes de nome de projetos de teste
  - Categorias de teste e organizacao
  - Localizacao de dados de teste e mocks" : ""}

${(PROJECT_TYPE == "React" || PROJECT_TYPE == "Angular" || PROJECT_TYPE == "Auto-detect") ? 
"#### UI Project Structure (if detected)

- **Component Organization**:
  - Patterns de estrutura de pastas de componentes
  - Estrategias de agrupamento (por feature, por tipo, etc.)
  - Componentes compartilhados vs. especificos

- **State Management**:
  - Organizacao de arquivos de estado
  - Estrutura de store global
  - Patterns de state local

- **Routing Organization**:
  - Localizacao de definicoes de rota
  - Organizacao de paginas/views
  - Tratamento de parametros de rota

- **API Integration**:
  - Organizacao do API client
  - Estrutura de service layer
  - Patterns de data fetching

- **Asset Management**:
  - Organizacao de assets estaticos
  - Estrutura de imagens/midias
  - Organizacao de fontes e icones
  
- **Style Organization**:
  - Estrutura de arquivos CSS/SCSS
  - Organizacao de temas
  - Patterns de modules de estilo" : ""}

### 4. File Placement Patterns

${INCLUDE_FILE_PATTERNS ? 
"Documente os patterns que determinam onde diferentes tipos de arquivos devem ficar:

- **Configuration Files**:
  - Localizacoes para diferentes tipos de configuracao
  - Patterns de configuracao por ambiente
  
- **Model/Entity Definitions**:
  - Onde modelos de dominio sao definidos
  - Localizacao de DTOs
  - Localizacao de schemas
  
- **Business Logic**:
  - Localizacao de implementacoes de services
  - Organizacao de regras de negocio
  - Localizacao de utilities e helpers
  
- **Interface Definitions**:
  - Onde interfaces e abstracoes sao definidas
  - Como interfaces sao agrupadas e organizadas
  
- **Test Files**:
  - Patterns de localizacao de testes unitarios
  - Localizacao de testes de integracao
  - Localizacao de utilitarios e mocks
  
- **Documentation Files**:
  - Localizacao de documentacao de API
  - Organizacao de documentacao interna
  - Distribuicao de arquivos README" : 
"Documente onde os tipos de arquivo-chave ficam no projeto."}

### 5. Naming and Organization Conventions
Documente as convencoes de nome e organizacao observadas no projeto:

- **File Naming Patterns**:
  - Convencoes de case (PascalCase, camelCase, kebab-case)
  - Prefixos e sufixos
  - Indicadores de tipo em nomes de arquivo
  
- **Folder Naming Patterns**:
  - Convencoes de nome para diferentes tipos de pasta
  - Patterns hierarquicos
  - Convencoes de agrupamento e categorizacao
  
- **Namespace/Module Patterns**:
  - Como namespaces/modules mapeiam para estrutura de pastas
  - Organizacao de import/using
  - Separacao de API interna vs publica

- **Organizational Patterns**:
  - Estrategias de co-localizacao de codigo
  - Abordagens de encapsulamento de features
  - Organizacao de concerns cross-cutting

### 6. Navigation and Development Workflow
Forneca orientacao para navegar e trabalhar com o codebase:

- **Entry Points**:
  - Entry points principais do app
  - Pontos iniciais de configuracao
  - Arquivos iniciais para entender o projeto

- **Common Development Tasks**:
  - Onde adicionar novas features
  - Como estender funcionalidades existentes
  - Onde colocar novos testes
  - Onde modificar configuracoes
  
- **Dependency Patterns**:
  - Como dependencias fluem entre pastas
  - Patterns de import/reference
  - Localizacoes de registro de dependency injection

${INCLUDE_FILE_COUNTS ? 
"- **Content Statistics**:
  - Analise de arquivos por diretorio
  - Metricas de distribuicao de codigo
  - Areas de concentracao de complexidade" : ""}

### 7. Build and Output Organization
Documente o processo de build e organizacao de outputs:

- **Build Configuration**:
  - Localizacoes e propositos de scripts de build
  - Organizacao de pipeline de build
  - Definicao de tarefas de build
  
- **Output Structure**:
  - Localizacao de outputs compilados/buildados
  - Patterns de organizacao de output
  - Estrutura de pacotes de distribuicao
  
- **Environment-Specific Builds**:
  - Diferencas entre dev e prod
  - Estrategias de configuracao por ambiente
  - Organizacao de variantes de build

### 8. Technology-Specific Organization

${(PROJECT_TYPE == ".NET" || PROJECT_TYPE == "Auto-detect") ? 
"#### .NET-Specific Structure Patterns (if detected)

- **Project File Organization**:
  - Estrutura e patterns de project file
  - Configuracao de target framework
  - Organizacao de property groups
  - Patterns de item groups
  
- **Assembly Organization**:
  - Patterns de nome de assemblies
  - Arquitetura multi-assembly
  - Patterns de referencia entre assemblies
  
- **Resource Organization**:
  - Patterns de embedded resources
  - Estrutura de arquivos de localizacao
  - Organizacao de static web assets
  
- **Package Management**:
  - Localizacao de configuracao NuGet
  - Organizacao de package references
  - Gestao de versoes de pacotes" : ""}

${(PROJECT_TYPE == "Java" || PROJECT_TYPE == "Auto-detect") ? 
"#### Java-Specific Structure Patterns (if detected)

- **Package Hierarchy**:
  - Convencoes de nome e nesting de packages
  - Packages de dominio vs tecnico
  - Patterns de visibilidade e acesso
  
- **Build Tool Organization**:
  - Estrutura e patterns Maven/Gradle
  - Organizacao de modulos
  - Patterns de configuracao de plugins
  
- **Resource Organization**:
  - Estrutura de pastas de resources
  - Resources por ambiente
  - Organizacao de properties" : ""}

${(PROJECT_TYPE == "Node.js" || PROJECT_TYPE == "Auto-detect") ? 
"#### Node.js-Specific Structure Patterns (if detected)

- **Module Organization**:
  - Organizacao CommonJS vs ESM
  - Patterns de modulos internos
  - Gestao de dependencias de terceiros
  
- **Script Organization**:
  - Patterns de scripts npm/yarn
  - Localizacao de scripts utilitarios
  - Scripts de ferramentas de desenvolvimento
  
- **Configuration Management**:
  - Localizacao de arquivos de configuracao
  - Gestao de variaveis de ambiente
  - Abordagens de gestao de secrets" : ""}

### 9. Extension and Evolution
Documente como a estrutura do projeto foi desenhada para evoluir:

- **Extension Points**:
  - Como adicionar novos modulos/features mantendo convencoes
  - Patterns de pasta para plugins/extensoes
  - Estruturas de customizacao
  
- **Scalability Patterns**:
  - Como a estrutura escala para features maiores
  - Abordagem para dividir modulos grandes
  - Estrategias de code splitting
  
- **Refactoring Patterns**:
  - Abordagens comuns de refatoracao
  - Como mudancas estruturais sao gerenciadas
  - Patterns de reorganizacao incremental

${INCLUDE_TEMPLATES ? 
"### 10. Structure Templates

Forneca templates para criar novos componentes seguindo convencoes do projeto:

- **New Feature Template**:
  - Estrutura de pastas para adicionar uma feature completa
  - Tipos de arquivo obrigatorios e suas localizacoes
  - Patterns de nome a seguir
  
- **New Component Template**:
  - Estrutura de diretorio para um componente tipico
  - Arquivos essenciais a incluir
  - Pontos de integracao com a estrutura existente
  
- **New Service Template**:
  - Estrutura para adicionar um novo service
  - Localizacao de interface e implementacao
  - Patterns de configuracao e registro
  
- **New Test Structure**:
  - Estrutura de pastas para testes
  - Templates de organizacao de arquivos de teste
  - Organizacao de resources de teste" : ""}

### ${INCLUDE_TEMPLATES ? "11" : "10"}. Structure Enforcement

Documente como a estrutura do projeto e mantida e aplicada:

- **Structure Validation**:
  - Ferramentas/scripts que reforcam estrutura
  - Checks de build para conformidade estrutural
  - Regras de linting relacionadas a estrutura
  
- **Documentation Practices**:
  - Como mudancas estruturais sao documentadas
