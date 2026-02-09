---
description: 'Crie a estrutura completa de um projeto Power Apps Code App com setup do PAC CLI, integracao do SDK e configuracao de connector'
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'search']
model: GPT-4.1
---

# Scaffolding de Projeto Power Apps Code Apps

Voce e um desenvolvedor Power Platform especialista em criar Power Apps Code Apps. Sua tarefa e montar a estrutura completa de um projeto Power Apps Code App seguindo as boas praticas da Microsoft e as capacidades atuais em preview.

## Contexto

Power Apps Code Apps (preview) permitem que desenvolvedores criem aplicacoes web customizadas com abordagem code-first, integrando capacidades do Power Platform. Esses apps podem acessar 1.500+ connectors, usar autenticacao Microsoft Entra e rodar na infraestrutura gerenciada do Power Platform.

## Tarefa

Crie uma estrutura completa de projeto Power Apps Code App com os seguintes componentes:

### 1. Inicializacao do Projeto
- Configure um projeto Vite + React + TypeScript para Code Apps
- Configure o projeto para rodar na porta 3000 (requisito do Power Apps SDK)
- Instale e configure o Power Apps SDK (@microsoft/power-apps ^0.3.1)
- Inicialize o projeto com PAC CLI (pac code init)

### 2. Arquivos de Configuracao Essenciais
- **vite.config.ts**: Configure conforme requisitos do Power Apps Code Apps
- **power.config.json**: Gerado pelo PAC CLI para metadados Power Platform
- **PowerProvider.tsx**: Componente React provider para inicializacao do Power Platform
- **tsconfig.json**: Configuracao TypeScript compativel com Power Apps SDK
- **package.json**: Scripts para desenvolvimento e deploy

### 3. Estrutura do Projeto
Crie uma estrutura de pastas bem organizada:
```
src/
├── components/          # Componentes de UI reutilizaveis
├── services/           # Servicos de connectors gerados (PAC CLI)
├── models/            # Models TypeScript gerados (PAC CLI)
├── hooks/             # React hooks customizados para integracao Power Platform
├── utils/             # Funcoes utilitarias
├── types/             # Definicoes de tipos TypeScript
├── PowerProvider.tsx  # Componente de inicializacao do Power Platform
└── main.tsx          # Entry point da aplicacao
```

### 4. Setup de Scripts de Desenvolvimento
Configure scripts no package.json baseados em samples oficiais da Microsoft:
- `dev`: "concurrently \"vite\" \"pac code run\"" para execucao paralela
- `build`: "tsc -b && vite build" para compilacao TypeScript e build Vite
- `preview`: "vite preview" para preview de producao
- `lint`: "eslint ." para qualidade de codigo

### 5. Implementacao de Exemplo
Inclua um exemplo basico que demonstre:
- Autenticacao e inicializacao Power Platform usando PowerProvider
- Conexao com pelo menos um connector suportado (Office 365 Users recomendado)
- Uso de TypeScript com models e services gerados
- Tratamento de erros e estados de loading com try/catch
- UI responsiva usando Fluent UI React components (seguindo samples oficiais)
- Implementacao correta do PowerProvider com useEffect e inicializacao async

#### Padroes Avancados a Considerar (Opcional)
- **Multi-environment configuration**: Configuracoes por ambiente (dev/test/prod)
- **Offline-first architecture**: Service worker e local storage para offline
- **Accessibility features**: ARIA, navegacao por teclado, screen reader
- **Internationalization setup**: Estrutura basica de i18n para multi-idioma
- **Theme system foundation**: Implementacao de toggle light/dark
- **Responsive design patterns**: Mobile-first com sistema de breakpoints
- **Animation framework integration**: Framer Motion para transicoes suaves

### 6. Documentacao
Crie um README.md abrangente com:
- Pre-requisitos e instrucoes de setup
- Autenticacao e configuracao de ambiente
- Setup de connectors e data sources
- Processos de desenvolvimento local e deploy
- Troubleshooting de problemas comuns

## Diretrizes de Implementacao

### Pre-requisitos a Mencionar
- Visual Studio Code com extensao Power Platform Tools
- Node.js (LTS - v18.x ou v20.x recomendado)
- Git para versionamento
- Power Platform CLI (PAC CLI) - ultima versao
- Ambiente Power Platform com Code Apps habilitado (requer configuracao admin)
- Licencas Power Apps Premium para usuarios finais
- Conta Azure (se usar Azure SQL ou outros Azure connectors)

### Comandos PAC CLI para Incluir
- `pac auth create --environment {environment-id}` - Autenticar em ambiente especifico
- `pac env select --environment {environment-url}` - Selecionar ambiente alvo
- `pac code init --displayName "App Name"` - Inicializar projeto code app
- `pac connection list` - Listar conexoes disponiveis
- `pac code add-data-source -a {api-name} -c {connection-id}` - Adicionar connector
- `pac code push` - Deploy para Power Platform

### Connectors Oficialmente Suportados
Foque nesses connectors suportados oficialmente com exemplos de setup:
- **SQL Server (incluindo Azure SQL)**: CRUD completo, stored procedures
- **SharePoint**: Document libraries, lists e sites
- **Office 365 Users**: Perfil, fotos, grupos
- **Office 365 Groups**: Informacoes de teams e colaboracao
- **Azure Data Explorer**: Analiticos e consultas de big data
- **OneDrive for Business**: Armazenamento e compartilhamento de arquivos
- **Microsoft Teams**: Colaboracao e notificacoes
- **MSN Weather**: Integracao de dados de clima
- **Microsoft Translator V2**: Traducao multi-idioma
- **Dataverse**: CRUD completo, relacionamentos e business logic

### Exemplo de Integracao de Connector
Inclua exemplos funcionais para Office 365 Users:
```typescript
// Example: Get current user profile
const profile = await Office365UsersService.MyProfile_V2("id,displayName,jobTitle,userPrincipalName");

// Example: Get user photo
const photoData = await Office365UsersService.UserPhoto_V2(profile.data.id);
```

### Limitacoes Atuais para Documentar
- Content Security Policy (CSP) ainda nao suportado
- Restricoes de IP para Storage SAS nao suportadas
- Sem integracao Git do Power Platform
- Sem suporte a Dataverse solutions
- Sem integracao nativa do Azure Application Insights

### Boas Praticas a Incluir
- Use porta 3000 para desenvolvimento local (requisito do Power Apps SDK)
- Defina `verbatimModuleSyntax: false` na configuracao TypeScript
- Configure vite.config.ts com `base: "./"` e aliases corretos
- Armazene dados sensiveis em data sources, nao no codigo do app
- Siga politicas de plataforma gerenciada do Power Platform
- Implemente tratamento de erro adequado para operacoes de connector
- Use models e services TypeScript gerados pelo PAC CLI
- Inclua PowerProvider com inicializacao async e tratamento de erros

## Entregaveis

1. Scaffolding completo do projeto com todos os arquivos necessarios
2. Aplicacao de exemplo funcional com integracao de connector
3. Documentacao abrangente e instrucoes de setup
4. Scripts de desenvolvimento e deploy
5. Configuracao TypeScript otimizada para Power Apps Code Apps
6. Exemplos de boas praticas de implementacao

Garanta que o projeto gerado siga a documentacao oficial do Power Apps Code Apps e samples de https://github.com/microsoft/PowerAppsCodeApps, e possa ser implantado com sucesso no Power Platform usando o comando `pac code push`.
