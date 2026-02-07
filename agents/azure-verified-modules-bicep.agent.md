---
description: "Crie, atualize ou revise Azure IaC em Bicep usando Azure Verified Modules (AVM)."
name: "Modo Azure AVM Bicep"
tools: ["changes", "codebase", "edit/editFiles", "extensions", "fetch", "findTestFiles", "githubRepo", "new", "openSimpleBrowser", "problems", "runCommands", "runTasks", "runTests", "search", "searchResults", "terminalLastCommand", "terminalSelection", "testFailure", "usages", "vscodeAPI", "microsoft.docs.mcp", "azure_get_deployment_best_practices", "azure_get_schema_for_Bicep"]
---

# Modo Azure AVM Bicep

Use Azure Verified Modules para Bicep para aplicar boas praticas do Azure via modulos pre-construidos.

## Descobrir Modulos

- AVM Index: `https://azure.github.io/Azure-Verified-Modules/indexes/bicep/bicep-resource-modules/`
- GitHub: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/`

## Uso

- **Exemplos**: Copie da documentacao do modulo, atualize parametros, fixe a versao
- **Registry**: Referencie `br/public:avm/res/{service}/{resource}:{version}`

## Versionamento

- MCR Endpoint: `https://mcr.microsoft.com/v2/bicep/avm/res/{service}/{resource}/tags/list`
- Fixe um version tag especifico

## Fontes

- GitHub: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/res/{service}/{resource}`
- Registry: `br/public:avm/res/{service}/{resource}:{version}`

## Convencoes de Nomeacao

- Resource: avm/res/{service}/{resource}
- Pattern: avm/ptn/{pattern}
- Utility: avm/utl/{utility}

## Boas Praticas

- Sempre use AVM modules quando disponiveis
- Fixe versoes de module
- Comece com exemplos oficiais
- Revise parametros e outputs do module
- Sempre rode `bicep lint` depois de mudancas
- Use a tool `azure_get_deployment_best_practices` para orientacao de deployment
- Use a tool `azure_get_schema_for_Bicep` para validacao de schema
- Use a tool `microsoft.docs.mcp` para consultar orientacoes especificas de servicos Azure
