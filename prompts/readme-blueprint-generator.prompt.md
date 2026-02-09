---
description: 'Prompt inteligente de geracao de README.md que analisa a estrutura de documentacao do projeto e cria documentacao abrangente do repositorio. Faz scan de arquivos em .github/copilot e copilot-instructions.md para extrair informacoes do projeto, stack, arquitetura, workflow de desenvolvimento, padroes de codigo e abordagem de testes, gerando documentacao markdown bem estruturada com formatacao adequada, cross-references e conteudo focado em desenvolvedores.'

agent: 'agent'
---

# Prompt de Geracao de README

Gere um README.md abrangente para este repositorio analisando os arquivos de documentacao na pasta .github/copilot e o arquivo copilot-instructions.md. Siga estes passos:

1. Faca scan de todos os arquivos na pasta .github/copilot, como:
   - Architecture
   - Code_Exemplars
   - Coding_Standards
   - Project_Folder_Structure
   - Technology_Stack
   - Unit_Tests
   - Workflow_Analysis

2. Revise tambem o arquivo copilot-instructions.md na pasta .github

3. Crie um README.md com as seguintes secoes:

## Nome e Descricao do Projeto
- Extraia o nome do projeto e o proposito principal da documentacao
- Inclua uma descricao concisa do que o projeto faz

## Stack de Tecnologia
- Liste as principais tecnologias, linguagens e frameworks usados
- Inclua versoes quando disponiveis
- Use como fonte principal o arquivo Technology_Stack

## Arquitetura do Projeto
- Forneca um overview de alto nivel da arquitetura
- Considere incluir um diagrama simples se descrito na documentacao
- Use como fonte o arquivo Architecture

## Primeiros Passos
- Inclua instrucoes de instalacao com base na stack
- Adicione passos de setup e configuracao
- Inclua pre-requisitos

## Estrutura do Projeto
- Overview breve da organizacao de pastas
- Use como fonte o arquivo Project_Folder_Structure

## Principais Funcionalidades
- Liste funcionalidades principais do projeto
- Extraia de varios arquivos de documentacao

## Workflow de Desenvolvimento
- Resuma o processo de desenvolvimento
- Inclua informacoes sobre branching strategy se disponivel
- Use como fonte o arquivo Workflow_Analysis

## Padroes de Codigo
- Resuma padroes e convencoes de codigo
- Use como fonte o arquivo Coding_Standards

## Testes
- Explique abordagem e ferramentas de teste
- Use como fonte o arquivo Unit_Tests

## Contribuicao
- Diretrizes para contribuir com o projeto
- Referencie code exemplars para orientacao
- Use como fonte Code_Exemplars e copilot-instructions

## Licenca
- Inclua informacoes de licenca se disponivel

Formate o README com Markdown adequado, incluindo:
- Titulos e subtitulos claros
- Blocos de codigo quando apropriado
- Listas para melhor legibilidade
- Links para outros arquivos de documentacao
- Badges para status de build, versao etc. se houver informacao

Mantenha o README conciso e informativo, focando no que novos desenvolvedores ou usuarios precisam saber sobre o projeto.
