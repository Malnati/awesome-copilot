---
agent: 'agent'
description: 'Crie um arquivo llms.txt do zero com base na estrutura do repositorio seguindo a especificacao llms.txt em https://llmstxt.org/'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'extensions', 'web/fetch', 'githubRepo', 'openSimpleBrowser', 'problems', 'runTasks', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# Criar arquivo LLMs.txt a partir da estrutura do repositorio

Crie um novo arquivo `llms.txt` do zero na raiz do repositorio seguindo a especificacao oficial em https://llmstxt.org/. Este arquivo fornece orientacao de alto nivel para large language models (LLMs) sobre onde encontrar conteudo relevante para entender o proposito e as especificacoes do repositorio.

## Diretriz Principal

Crie um `llms.txt` abrangente que sirva como ponto de entrada para LLMs entenderem e navegarem no repositorio. O arquivo deve obedecer a especificacao llms.txt e ser otimizado para consumo por LLMs, mantendo legibilidade humana.

## Fase de Analise e Planejamento

Antes de criar o `llms.txt`, voce deve concluir uma analise completa:

### Etapa 1: Revisar a especificacao llms.txt

- Revise a especificacao oficial em https://llmstxt.org/ para garantir conformidade total
- Entenda o formato exigido e as diretrizes
- Anote requisitos de estrutura Markdown

### Etapa 2: Analise da Estrutura do Repositorio

- Examine toda a estrutura do repositorio usando as ferramentas adequadas
- Identifique o proposito principal e o escopo do repositorio
- Catalogue diretorios importantes e seus propositos
- Liste arquivos-chave valiosos para entendimento por LLM

### Etapa 3: Descoberta de Conteudo

- Identifique arquivos README e suas localizacoes
- Encontre arquivos de documentacao (`.md` em `/docs/`, `/spec/`, etc.)
- Localize arquivos de especificacao e seus propositos
- Descubra arquivos de configuracao e sua relevancia
- Encontre exemplos e code samples
- Identifique qualquer estrutura de documentacao existente

### Etapa 4: Criar Plano de Implementacao

Com base na analise, crie um plano estruturado que inclua:

- Resumo do proposito e escopo do repositorio
- Lista priorizada de arquivos essenciais para entendimento por LLM
- Arquivos secundarios que fornecem contexto adicional
- Estrutura organizacional para o llms.txt

## Requisitos de Implementacao

### Conformidade de Formato

O `llms.txt` deve seguir esta estrutura exata:

1. **H1 Header**: Linha unica com nome do repositorio/projeto (obrigatorio)
2. **Blockquote Summary**: Descricao breve em formato de blockquote (opcional mas recomendado)
3. **Additional Details**: Zero ou mais secoes Markdown sem headings para contexto
4. **File List Sections**: Zero ou mais secoes H2 contendo listas Markdown de links

### Requisitos de Conteudo

#### Elementos Obrigatorios

- **Project Name**: Titulo claro e descritivo em H1
- **Summary**: Blockquote conciso explicando o proposito do repositorio
- **Key Files**: Arquivos essenciais organizados por categoria (secoes H2)

#### Formato de Link

Cada link deve seguir: `[descriptive-name](relative-url): optional description`

#### Organizacao de Secoes

Organize arquivos em secoes H2 como:

- **Documentation**: Documentacao principal
- **Specifications**: Especificacoes tecnicas e requisitos
- **Examples**: Codigo exemplo e uso
- **Configuration**: Arquivos de setup e configuracao
- **Optional**: Arquivos secundarios (significado especial - podem ser pulados para contexto menor)

### Diretrizes de Conteudo

#### Linguagem e Estilo

- Use linguagem concisa, clara e sem ambiguidades
- Evite jargao sem explicacao
- Escreva para humanos e LLMs
- Seja especifico e informativo nas descricoes

#### Criterios de Selecao de Arquivos

Inclua arquivos que:
- Expliquem o proposito e escopo do repositorio
- Fornecam documentacao tecnica essencial
- Mostrem exemplos e patterns de uso
- Definam interfaces e especificacoes
- Contenham instrucoes de setup e configuracao

Exclua arquivos que:
- Sao apenas detalhes de implementacao
- Contenham informacao redundante
- Sao artefatos de build ou conteudo gerado
- Nao sao relevantes para entender o projeto

## Etapas de Execucao

### Etapa 1: Analise do Repositorio

1. Examine a estrutura do repositorio completamente
2. Leia o README.md principal para entender o projeto
3. Identifique diretorios e arquivos de documentacao
4. Catalogue arquivos de especificacao e seus propositos
5. Encontre exemplos e arquivos de configuracao

### Etapa 2: Planejamento de Conteudo

1. Determine a declaracao principal de proposito
2. Escreva um resumo conciso para o blockquote
3. Agrupe arquivos em categorias logicas
4. Priorize arquivos por importancia para entendimento por LLM
5. Crie descricoes para cada link

### Etapa 3: Criacao do Arquivo

1. Crie o arquivo `llms.txt` na raiz do repositorio
2. Siga a especificacao de formato exata
3. Inclua todas as secoes obrigatorias
4. Use formatacao Markdown correta
5. Garanta que todos os links sejam caminhos relativos validos

### Etapa 4: Validacao
1. Verifique conformidade com a especificacao em https://llmstxt.org/
2. Cheque se todos os links sao validos e acessiveis
3. Garanta que o arquivo funcione como ferramenta de navegacao para LLM
4. Confirme que o arquivo e legivel por humanos e maquinas

## Quality Assurance

### Validacao de Formato

- ✅ H1 header com nome do projeto
- ✅ Blockquote summary (se incluido)
- ✅ Secoes H2 para listas de arquivos
- ✅ Formato correto de links Markdown
- ✅ Sem links quebrados ou invalidos
- ✅ Formatacao consistente em todo o arquivo

### Validacao de Conteudo

- ✅ Linguagem clara e sem ambiguidades
- ✅ Cobertura abrangente de arquivos essenciais
- ✅ Organizacao logica de conteudo
- ✅ Descricoes apropriadas de arquivos
- ✅ Serve como ferramenta efetiva de navegacao para LLM

### Conformidade com Especificacao

- ✅ Segue exatamente o formato de https://llmstxt.org/
- ✅ Usa estrutura Markdown exigida
- ✅ Implementa secoes opcionais apropriadamente
- ✅ Arquivo localizado na raiz do repositorio (`/llms.txt`)

## Template de Estrutura de Exemplo

```txt
# [Repository Name]

> [Concise description of the repository's purpose and scope]

[Optional additional context paragraphs without headings]

## Documentation

- [Main README](README.md): Primary project documentation and getting started guide
- [Contributing Guide](CONTRIBUTING.md): Guidelines for contributing to the project
- [Code of Conduct](CODE_OF_CONDUCT.md): Community guidelines and expectations

## Specifications

- [Technical Specification](spec/technical-spec.md): Detailed technical requirements and constraints
- [API Specification](spec/api-spec.md): Interface definitions and data contracts

## Examples

- [Basic Example](examples/basic-usage.md): Simple usage demonstration
- [Advanced Example](examples/advanced-usage.md): Complex implementation patterns

## Configuration

- [Setup Guide](docs/setup.md): Installation and configuration instructions
- [Deployment Guide](docs/deployment.md): Production deployment guidelines

## Optional

- [Architecture Documentation](docs/architecture.md): Detailed system architecture
- [Design Decisions](docs/decisions.md): Historical design decision records
```

## Success Criteria

O arquivo `llms.txt` criado deve:
1. Permitir que LLMs entendam rapidamente o proposito do repositorio
2. Fornecer navegacao clara para documentacao essencial
3. Seguir exatamente a especificacao llms.txt oficial
4. Ser abrangente e conciso
5. Servir efetivamente leitores humanos e maquinas
6. Incluir todos os arquivos criticos para entendimento do projeto
7. Usar linguagem clara e sem ambiguidades
8. Organizar o conteudo de forma logica para consumo facil
