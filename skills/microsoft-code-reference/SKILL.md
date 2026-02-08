---
name: microsoft-code-reference
description: Consulte referencias de API da Microsoft, encontre amostras de codigo funcionais e valide se o codigo do SDK esta correto. Use ao trabalhar com SDKs Azure, bibliotecas .NET ou APIs Microsoft — para encontrar o metodo certo, checar parametros, obter exemplos funcionais ou solucionar erros. Evita metodos alucinados, assinaturas erradas e padroes obsoletos consultando docs oficiais.
compatibility: Requer Microsoft Learn MCP Server (https://learn.microsoft.com/api/mcp)
---

# Referencia de Codigo da Microsoft

## Ferramentas (Tools)

| Necessidade | Ferramenta (Tool) | Exemplo |
|------|------|---------|
| Buscar metodo/classe de API | `microsoft_docs_search` | `"BlobClient UploadAsync Azure.Storage.Blobs"` |
| Amostra de codigo funcional | `microsoft_code_sample_search` | `query: "upload blob managed identity", language: "python"` |
| Referencia completa de API | `microsoft_docs_fetch` | Buscar URL de `microsoft_docs_search` (overloads, assinaturas completas) |

## Encontrar Amostras de Codigo

Use `microsoft_code_sample_search` para obter exemplos oficiais e funcionais:

```
microsoft_code_sample_search(query: "upload file to blob storage", language: "csharp")
microsoft_code_sample_search(query: "authenticate with managed identity", language: "python")
microsoft_code_sample_search(query: "send message service bus", language: "javascript")
```

**Quando usar:**
- Antes de escrever codigo — encontre um padrao funcional para seguir
- Depois de erros — compare seu codigo com um exemplo conhecido
- Em duvida sobre inicializacao/setup — exemplos mostram contexto completo

## Buscas de API

```
# Verifique se o metodo existe (inclua namespace para precisao)
"BlobClient UploadAsync Azure.Storage.Blobs"
"GraphServiceClient Users Microsoft.Graph"

# Encontrar classe/interface
"DefaultAzureCredential class Azure.Identity"

# Encontrar pacote correto
"Azure Blob Storage NuGet package"
"azure-storage-blob pip package"
```

Busque a pagina completa quando o metodo tiver multiplos overloads ou quando precisar de detalhes completos de parametros.

## Solucao de Problemas de Erros

Use `microsoft_code_sample_search` para encontrar amostras funcionais e comparar com sua implementacao. Para erros especificos, use `microsoft_docs_search` e `microsoft_docs_fetch`:

| Tipo de Erro | Query (Consulta) |
|------------|-------|
| Metodo nao encontrado | `"[ClassName] methods [Namespace]"` |
| Tipo nao encontrado | `"[TypeName] NuGet package namespace"` |
| Assinatura errada | `"[ClassName] [MethodName] overloads"` → buscar pagina completa |
| Aviso deprecado | `"[OldType] migration v12"` |
| Falha de auth | `"DefaultAzureCredential troubleshooting"` |
| 403 Forbidden | `"[ServiceName] RBAC permissions"` |

## Quando Verificar

Sempre verifique quando:
- Nome de metodo parece "conveniente demais" (`UploadFile` vs `Upload` real)
- Mistura versoes de SDK (v11 `CloudBlobClient` vs v12 `BlobServiceClient`)
- Nome de pacote nao segue convencoes (`Azure.*` para .NET, `azure-*` para Python)
- Usa uma API pela primeira vez

## Workflow de Validacao (Validation)

Antes de gerar codigo usando SDKs Microsoft, valide se esta correto:

1. **Confirmar metodo ou pacote existe** — `microsoft_docs_search(query: "[ClassName] [MethodName] [Namespace]")`
2. **Buscar detalhes completos** (para overloads/params complexos) — `microsoft_docs_fetch(url: "...")`
3. **Encontrar amostra funcional** — `microsoft_code_sample_search(query: "[task]", language: "[lang]")`

Para buscas simples, o passo 1 pode bastar. Para uso complexo de API, complete os tres passos.
