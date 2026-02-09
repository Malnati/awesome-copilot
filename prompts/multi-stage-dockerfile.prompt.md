---
agent: 'agent'
tools: ['search/codebase']
description: 'Crie Dockerfiles multi-stage otimizados para qualquer linguagem ou framework'
---

Seu objetivo e me ajudar a criar Dockerfiles multi-stage eficientes que sigam boas praticas, resultando em imagens menores e mais seguras.

## Estrutura Multi-Stage

- Use um stage builder para compilacao, instalacao de dependencias e outras operacoes de build
- Use um stage de runtime separado que inclua apenas o necessario para rodar a aplicacao
- Copie apenas os artefatos necessarios do stage builder para o stage runtime
- Use nomes significativos para stages com a palavra-chave `AS` (por exemplo, `FROM node:18 AS builder`)
- Organize stages em ordem logica: dependencias → build → test → runtime

## Imagens Base

- Comece com imagens oficiais e minimas quando possivel
- Especifique tags de versao exatas para builds reproduziveis (por exemplo, `python:3.11-slim` e nao apenas `python`)
- Considere imagens distroless para stages de runtime quando apropriado
- Use imagens baseadas em Alpine para menor tamanho quando compativeis com sua aplicacao
- Garanta que a imagem de runtime tenha apenas dependencias minimas necessarias

## Otimizacao de Layers

- Organize comandos para maximizar cache de layers
- Coloque comandos que mudam frequentemente (mudancas de codigo) apos comandos que mudam menos (instalacao de dependencias)
- Use `.dockerignore` para evitar arquivos desnecessarios no contexto de build
- Combine comandos RUN relacionados com `&&` para reduzir quantidade de layers
- Considere usar COPY --chown para definir permissoes em um unico passo

## Praticas de Seguranca

- Evite rodar containers como root - use instrucao `USER` para usuario nao-root
- Remova ferramentas de build e pacotes desnecessarios da imagem final
- Faça scan da imagem final por vulnerabilidades
- Defina permissoes de arquivo restritivas
- Use multi-stage builds para evitar incluir secrets de build na imagem final

## Consideracoes de Performance

- Use build arguments para configuracoes que podem mudar entre ambientes
- Aproveite build cache ordenando layers do menos para o mais frequente
- Considere paralelizacao em etapas de build quando possivel
- Defina variaveis de ambiente apropriadas como NODE_ENV=production para otimizar runtime
- Use healthchecks apropriados para o tipo de aplicacao com a instrucao HEALTHCHECK
