---
agent: 'agent'
description: 'Gere uma aplicacao completa e pronta para producao a partir de uma especificacao OpenAPI'
model: 'GPT-4.1'
tools: ['codebase', 'edit/editFiles', 'search/codebase']
---

# Gerar Aplicacao a partir de OpenAPI Spec

Seu objetivo e gerar uma aplicacao completa e funcional a partir de uma especificacao OpenAPI usando as convencoes e boas praticas do framework ativo.

## Requisitos de Input

1. **Especificacao OpenAPI**: Forneca um destes:
   - URL do OpenAPI spec (por exemplo, `https://api.example.com/openapi.json`)
   - Caminho local para o OpenAPI spec
   - O conteudo completo da especificacao OpenAPI colado diretamente

2. **Detalhes do Projeto** (se nao estiverem no spec):
   - Nome e descricao do projeto
   - Framework alvo e versao
   - Convencoes de nome de package/namespace
   - Metodo de autenticacao (se nao especificado no OpenAPI)

## Processo de Geracao

### Etapa 1: Analisar a Especificacao OpenAPI
- Validar o OpenAPI spec quanto a completude e corretude
- Identificar todos os endpoints, metodos HTTP e schemas de request/response
- Extrair requisitos de autenticacao e security schemes
- Anotar relacoes e restricoes do modelo de dados
- Sinalizar ambiguidades ou definicoes incompletas

### Etapa 2: Desenhar Arquitetura da Aplicacao
- Planejar a estrutura de diretorios apropriada para o framework
- Identificar agrupamento de controllers/handlers por resource ou dominio
- Desenhar organizacao da service layer para logica de negocio
- Planejar modelos de dados e relacionamentos de entidades
- Desenhar estrategia de configuracao e inicializacao

### Etapa 3: Gerar Codigo da Aplicacao
- Criar a estrutura de projeto com arquivos de build/package
- Gerar models/DTOs a partir de schemas OpenAPI
- Gerar controllers/handlers com mapeamento de rotas
- Gerar service layer com logica de negocio
- Gerar camada de repository/data access se aplicavel
- Adicionar tratamento de erros, validacao e logging
- Gerar configuracao e codigo de startup

### Etapa 4: Adicionar Arquivos de Suporte
- Gerar testes unitarios apropriados para services e controllers
- Criar README com instrucoes de setup e execucao
- Adicionar .gitignore e templates de configuracao de ambiente
- Gerar arquivos de documentacao da API
- Criar requests de exemplo/testes de integracao

## Estrutura de Saida

A aplicacao gerada incluira:

```
project-name/
├── README.md                      # Setup and usage instructions
├── [build-config]                 # Framework-specific build files (pom.xml, build.gradle, package.json, etc.)
├── src/
│   ├── main/
│   │   ├── [language]/
│   │   │   ├── controllers/       # HTTP endpoint handlers
│   │   │   ├── services/          # Business logic
│   │   │   ├── models/            # Data models and DTOs
│   │   │   ├── repositories/      # Data access (if applicable)
│   │   │   └── config/            # Application configuration
│   │   └── resources/             # Configuration files
│   └── test/
│       ├── [language]/
│       │   ├── controllers/       # Controller tests
│       │   └── services/          # Service tests
│       └── resources/             # Test configuration
├── .gitignore
├── .env.example                   # Environment variables template
└── docker-compose.yml             # Optional: Docker setup (if applicable)
```

## Boas Praticas Aplicadas

- **Framework Conventions**: Segue convencoes, estrutura e padroes do framework
- **Separation of Concerns**: Camadas claras com controllers, services e repositories
- **Error Handling**: Tratamento de erros abrangente com respostas significativas
- **Validation**: Validacao de input e schema ao longo do codigo
- **Logging**: Logging estruturado para debug e monitoramento
- **Testing**: Testes unitarios para services e controllers
- **Documentation**: Documentacao inline e instrucoes de setup
- **Security**: Implementa autenticacao/autorizacao do OpenAPI spec
- **Scalability**: Padroes de design suportam crescimento e manutencao

## Proximos Passos

Apos a geracao:

1. Revise a estrutura de codigo gerada e ajuste conforme necessario
2. Instale dependencias de acordo com o framework
3. Configure variaveis de ambiente e conexoes de banco
4. Rode testes para verificar o codigo gerado
5. Inicie o servidor de desenvolvimento
6. Teste endpoints usando os exemplos fornecidos

## Perguntas se Necessario

- A aplicacao deve incluir setup de banco/ORM ou apenas dados em memoria/mock?
- Voce quer configuracao Docker para containerizacao?
- A autenticacao deve ser JWT, OAuth2, API keys ou basic auth?
- Voce precisa de testes de integracao ou apenas testes unitarios?
- Alguma preferencia de tecnologia de banco?
- A API deve incluir exemplos de paginacao, filtering e sorting?
