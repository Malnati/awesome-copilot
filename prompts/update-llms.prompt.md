---
agent: 'agent'
description: 'Atualize o arquivo llms.txt na pasta raiz para refletir mudancas na documentacao ou especificacoes seguindo a especificacao llms.txt em https://llmstxt.org/'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'extensions', 'web/fetch', 'githubRepo', 'openSimpleBrowser', 'problems', 'runTasks', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# Atualizar Arquivo LLMs.txt

Atualize o arquivo `llms.txt` existente na raiz do repositorio para refletir mudancas na documentacao, especificacoes ou estrutura do repositorio. Este arquivo fornece orientacao de alto nivel para large language models (LLMs) sobre onde encontrar conteudo relevante para entender o proposito e as especificacoes do repositorio.

## Diretriz Primaria

Atualize o arquivo `llms.txt` existente para manter precisao e conformidade com a especificacao llms.txt enquanto reflete a estrutura e o conteudo atuais do repositorio. O arquivo deve permanecer otimizado para consumo por LLMs enquanto permanece legivel para humanos.

## Fase de Analise e Planejamento

Antes de atualizar o arquivo `llms.txt`, voce deve concluir uma analise completa:

### Etapa 1: Revisar Arquivo Atual e Especificacao
- Leia o arquivo `llms.txt` existente para entender a estrutura atual
- Revise a especificacao oficial em https://llmstxt.org/ para garantir conformidade continua
- Identifique areas que podem precisar de atualizacoes com base em mudancas no repositorio

### Etapa 2: Analise da Estrutura do Repositorio
- Examine a estrutura atual do repositorio usando ferramentas apropriadas
- Compare a estrutura atual com o que esta documentado no `llms.txt` existente
- Identifique novos diretorios, arquivos ou documentacao que devem ser incluidos
- Observe arquivos removidos ou realocados que precisam ser atualizados

### Etapa 3: Descoberta de Conteudo e Deteccao de Mudancas
- Identifique novos arquivos README e suas localizacoes
- Encontre novos arquivos de documentacao (arquivos `.md` em `/docs/`, `/spec/`, etc.)
- Localize novos arquivos de especificacao e seus propositos
- Descubra novos arquivos de configuracao e sua relevancia
- Encontre novos arquivos de exemplo e amostras de codigo
- Identifique quaisquer mudancas na estrutura de documentacao existente

### Etapa 4: Criar Plano de Atualizacao
Com base na sua analise, crie um plano estruturado que inclua:
- Mudancas necessarias para manter precisao
- Novos arquivos a serem adicionados ao llms.txt
- Referencias desatualizadas a serem removidas ou atualizadas
- Melhorias organizacionais para manter a clareza

## Requisitos de Implementacao

### Conformidade de Formato
O arquivo `llms.txt` atualizado deve manter esta estrutura exata conforme a especificacao:

1. **H1 Header**: Linha unica com o nome do repositorio/projeto (obrigatorio)
2. **Blockquote Summary**: Breve descricao em formato de blockquote (opcional, mas recomendado)
3. **Additional Details**: Zero ou mais secoes de markdown sem cabecalhos para contexto
4. **File List Sections**: Zero ou mais secoes H2 contendo listas de links em markdown

### Requisitos de Conteudo

#### Elementos Obrigatorios
- **Project Name**: Titulo claro e descritivo como H1
- **Summary**: Blockquote conciso explicando o proposito do repositorio
- **Key Files**: Arquivos essenciais organizados por categoria (secoes H2)

#### Formato de Link de Arquivo
Cada link de arquivo deve seguir: `[descriptive-name](relative-url): optional description`

#### Organizacao de Secoes
Organize arquivos em secoes H2 logicas como:
- **Documentation**: Arquivos centrais de documentacao
- **Specifications**: Especificacoes tecnicas e requisitos
- **Examples**: Codigo de exemplo e exemplos de uso
- **Configuration**: Arquivos de configuracao e setup
- **Optional**: Arquivos secundarios (significado especial - pode ser omitido para contexto mais curto)

### Diretrizes de Conteudo

#### Linguagem e Estilo
- Use linguagem concisa, clara e sem ambiguidades
- Evite jargao sem explicacao
- Escreva para leitores humanos e LLMs
- Seja especifico e informativo nas descricoes

#### Criterios de Selecao de Arquivos
Inclua arquivos que:
- Expliquem o proposito e o escopo do repositorio
- Fornecam documentacao tecnica essencial
- Mostrem exemplos de uso e padroes
- Definam interfaces e especificacoes
- Contenham instrucoes de configuracao e setup

Exclua arquivos que:
- Sejam apenas detalhes de implementacao
- Contenham informacao redundante
- Sejam artifacts de build ou conteudo gerado
- Nao sejam relevantes para entender o projeto

## Etapas de Execucao

### Etapa 1: Analise do Estado Atual
1. Leia o arquivo `llms.txt` existente completamente
2. Examine toda a estrutura atual do repositorio
3. Compare referencias de arquivo existentes com o conteudo real do repositorio
4. Identifique referencias desatualizadas, ausentes ou incorretas
5. Observe quaisquer problemas estruturais no arquivo atual

### Etapa 2: Planejamento de Conteudo
1. Determine se a declaracao de proposito principal precisa de atualizacoes
2. Revise e atualize o blockquote de resumo, se necessario
3. Planeje adicoes para novos arquivos e diretorios
4. Planeje remocoes para conteudo desatualizado ou movido
5. Reorganize secoes, se necessario, para melhor clareza

### Etapa 3: Atualizacoes de Arquivo
1. Atualize o arquivo `llms.txt` existente na raiz do repositorio
2. Mantenha conformidade com a especificacao exata de formato
3. Adicione novas referencias de arquivo com descricoes apropriadas
4. Remova ou atualize referencias desatualizadas
5. Garanta que todos os links sejam caminhos relativos validos

### Etapa 4: Validacao
1. Verifique conformidade continua com a especificacao https://llmstxt.org/
2. Verifique se todos os links sao validos e acessiveis
3. Garanta que o arquivo ainda serve como ferramenta eficaz de navegacao para LLMs
4. Confirme que o arquivo permanece legivel tanto por humanos quanto por maquinas

## Garantia de Qualidade

### Validacao de Formato
- ✅ H1 header com nome do projeto
- ✅ Blockquote summary (se incluido)
- ✅ Secoes H2 para listas de arquivos
- ✅ Formato correto de link em markdown
- ✅ Sem links quebrados ou invalidos
- ✅ Formatacao consistente em todo o arquivo

### Validacao de Conteudo
- ✅ Linguagem clara e sem ambiguidades
- ✅ Cobertura abrangente de arquivos essenciais
- ✅ Organizacao logica do conteudo
- ✅ Descricoes de arquivos apropriadas
- ✅ Serve como ferramenta eficaz de navegacao para LLMs

### Conformidade com Especificacao
- ✅ Segue exatamente o formato de https://llmstxt.org/
- ✅ Usa estrutura markdown obrigatoria
- ✅ Implementa secoes opcionais adequadamente
- ✅ Arquivo localizado na raiz do repositorio (`/llms.txt`)

## Estrategia de Atualizacao

### Processo de Adicao
Ao adicionar novo conteudo:
1. Identifique a secao apropriada para novos arquivos
2. Crie nomes claros e descritivos para os links
3. Escreva descricoes concisas, mas informativas
4. Mantenha ordenacao alfabetica ou logica dentro das secoes
5. Considere se novas secoes sao necessarias para novos tipos de conteudo

### Processo de Remocao
Ao remover conteudo desatualizado:
1. Verifique se os arquivos foram realmente removidos ou realocados
2. Verifique se arquivos realocados devem ser atualizados em vez de removidos
3. Remova secoes inteiras se ficarem vazias
4. Atualize referencias cruzadas, se necessario

### Processo de Reorganizacao
Ao reestruturar conteudo:
1. Mantenha fluxo logico do geral para o especifico
2. Mantenha documentacao essencial nas secoes primarias
3. Mova conteudo secundario para a secao "Optional" se apropriado
4. Garanta que a nova organizacao melhore a navegacao para LLMs

Exemplo de estrutura para `llms.txt`:

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

## Criterios de Sucesso

O arquivo `llms.txt` atualizado deve:
1. Refletir com precisao a estrutura e o conteudo atuais do repositorio
2. Manter conformidade com a especificacao llms.txt
3. Fornecer navegacao clara para documentacao essencial
4. Remover referencias desatualizadas ou incorretas
5. Incluir novos arquivos e documentacao importantes
6. Manter organizacao logica para consumo facil por LLMs
7. Usar linguagem clara e sem ambiguidades em todo o arquivo
8. Continuar a servir leitores humanos e de maquina de forma eficaz
