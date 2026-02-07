---
name: microsoft-docs
description: Consulte documentacao oficial da Microsoft para entender conceitos, encontrar tutoriais e aprender como servicos funcionam. Use para Azure, .NET, Microsoft 365, Windows, Power Platform e todas as tecnologias Microsoft. Obtenha informacoes precisas e atuais do learn.microsoft.com e outros sites oficiais da Microsoft — visoes de arquitetura, quickstarts, guias de configuracao, limites e boas praticas.
compatibility: Requer Microsoft Learn MCP Server (https://learn.microsoft.com/api/mcp)
---

# Microsoft Docs

## Tools

| Tool | Use For |
|------|---------|
| `microsoft_docs_search` | Encontrar documentacao — conceitos, guias, tutoriais, configuracao |
| `microsoft_docs_fetch` | Obter conteudo completo da pagina (quando trechos de busca nao bastam) |

## Quando Usar

- **Entender conceitos** — "How does Cosmos DB partitioning work?"
- **Aprender um servico** — "Azure Functions overview", "Container Apps architecture"
- **Encontrar tutoriais** — "quickstart", "getting started", "step-by-step"
- **Opcoes de configuracao** — "App Service configuration settings"
- **Limites e quotas** — "Azure OpenAI rate limits", "Service Bus quotas"
- **Boas praticas** — "Azure security best practices"

## Eficacia de Queries

Boas queries sao especificas:

```
# ❌ Too broad
"Azure Functions"

# ✅ Specific
"Azure Functions Python v2 programming model"
"Cosmos DB partition key design best practices"
"Container Apps scaling rules KEDA"
```

Inclua contexto:
- **Versao** quando relevante (`.NET 8`, `EF Core 8`)
- **Intencao da tarefa** (`quickstart`, `tutorial`, `overview`, `limits`)
- **Plataforma** para docs multiplataforma (`Linux`, `Windows`)

## Quando Buscar a Pagina Completa

Busque apos o search quando:
- **Tutoriais** — precisa de instrucoes completas passo a passo
- **Guias de configuracao** — precisa de todas as opcoes listadas
- **Deep dives** — usuario quer cobertura abrangente
- **Trecho de busca cortado** — contexto completo necessario

## Por Que Usar

- **Precisao** — docs atuais, nao dados de treinamento desatualizados
- **Completude** — tutoriais tem todos os passos, nao fragmentos
- **Autoridade** — documentacao oficial Microsoft
