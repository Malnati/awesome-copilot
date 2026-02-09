---
name: appinsights-instrumentation
description: 'Instrumente uma webapp para enviar dados de telemetria uteis ao Azure App Insights'
---

# Instrumentacao do App Insights (AppInsights)

Esta skill habilita o envio de dados de telemetria de uma webapp para o Azure App Insights para melhor observabilidade da saude do app.

## Quando usar esta skill

Use esta skill quando o usuario quiser habilitar telemetria para sua webapp.

## Pre-requisitos

O app no workspace deve ser de um destes tipos:

- Um app ASP.NET Core hospedado no Azure
- Um app Node.js hospedado no Azure

## Diretrizes

### Coletar informacoes de contexto

Descubra a tupla (linguagem de programacao, framework da aplicacao, hospedagem) do app para o qual o usuario quer adicionar suporte de telemetria. Isso determina como o app pode ser instrumentado. Leia o codigo-fonte para fazer uma inferencia informada. Confirme com o usuario qualquer coisa que voce nao saiba. Voce deve sempre perguntar ao usuario onde o app esta hospedado (ex.: em um computador pessoal, em um Azure App Service como codigo, em um Azure App Service como container, em um Azure Container App, etc.).

### Preferir auto-instrumentacao se possivel

Se o app for um app C# ASP.NET Core hospedado no Azure App Service, use [AUTO guide](references/AUTO.md) para ajudar o usuario a auto-instrumentar o app.

### Instrumentar manualmente

Instrumente manualmente o app criando o recurso AppInsights e atualizando o codigo do app.

#### Criar recurso App Insights (AppInsights)

Use uma das opcoes abaixo que se encaixe no ambiente.

- Adicione AppInsights ao template Bicep existente. Veja [examples/appinsights.bicep](examples/appinsights.bicep) para o que adicionar. Esta e a melhor opcao se houver arquivos de template Bicep no workspace.
- Use Azure CLI. Veja [scripts/appinsights.ps1](scripts/appinsights.ps1) para o comando Azure CLI a ser executado para criar o recurso App Insights.

Independentemente da opcao escolhida, recomende ao usuario criar o recurso App Insights em um resource group significativo que facilite o gerenciamento de recursos. Um bom candidato sera o mesmo resource group que contem os recursos do app hospedado no Azure.

#### Modificar o codigo da aplicacao

- Se o app for ASP.NET Core, veja [ASPNETCORE guide](references/ASPNETCORE.md) para como modificar o codigo C#.
- Se o app for Node.js, veja [NODEJS guide](references/NODEJS.md) para como modificar o codigo JavaScript/TypeScript.
- Se o app for Python, veja [PYTHON guide](references/PYTHON.md) para como modificar o codigo Python.
