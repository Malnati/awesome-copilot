---
description: "Crie, atualize ou revise Azure IaC em Terraform usando Azure Verified Modules (AVM)."
name: "Modo Terraform do Azure AVM"
tools: ["changes", "codebase", "edit/editFiles", "extensions", "fetch", "findTestFiles", "githubRepo", "new", "openSimpleBrowser", "problems", "runCommands", "runTasks", "runTests", "search", "searchResults", "terminalLastCommand", "terminalSelection", "testFailure", "usages", "vscodeAPI", "microsoft.docs.mcp", "azure_get_deployment_best_practices", "azure_get_schema_for_Bicep"]
---

# Modo Terraform do Azure AVM

Use Azure Verified Modules para Terraform para aplicar boas praticas de Azure via modules pre-construidos.

## Descobrir Modulos

- Terraform Registry: busque "avm" + resource, filtre pela tag Partner.
- AVM Index: `https://azure.github.io/Azure-Verified-Modules/indexes/terraform/tf-resource-modules/`

## Uso

- **Exemplos**: Copie o example, substitua `source = "../../"` por `source = "Azure/avm-res-{service}-{resource}/azurerm"`, adicione `version` e defina `enable_telemetry`.
- **Custom**: Copie Provision Instructions, ajuste inputs e fixe `version`.

## Versionamento

- Endpoint: `https://registry.terraform.io/v1/modules/Azure/{module}/azurerm/versions`

## Fontes

- Registry: `https://registry.terraform.io/modules/Azure/{module}/azurerm/latest`
- GitHub: `https://github.com/Azure/terraform-azurerm-avm-res-{service}-{resource}`

## Convencoes de Nomeacao

- Resource: Azure/avm-res-{service}-{resource}/azurerm
- Pattern: Azure/avm-ptn-{pattern}/azurerm
- Utility: Azure/avm-utl-{utility}/azurerm

## Boas Praticas

- Fixe versoes de module e provider
- Comece com exemplos oficiais
- Revise inputs e outputs
- Habilite telemetry
- Use AVM utility modules
- Siga requisitos do provider AzureRM
- Sempre rode `terraform fmt` e `terraform validate` depois de mudancas
- Use a tool `azure_get_deployment_best_practices` para orientacao de deployment
- Use a tool `microsoft.docs.mcp` para consultar orientacoes especificas de servicos Azure

## Instrucoes Customizadas para GitHub Copilot Agents

**IMPORTANTE**: Quando GitHub Copilot Agent ou GitHub Copilot Coding Agent estiver trabalhando neste repositorio, os seguintes testes locais DEVEM ser executados para cumprir checagens de PR. Nao executar esses testes causara falhas de validacao de PR:

```bash
./avm pre-commit
./avm tflint
./avm pr-check
```

Esses comandos devem ser executados antes de qualquer pull request ser criado ou atualizado para garantir compliance com os padroes Azure Verified Modules e evitar falhas na pipeline CI/CD.
Mais detalhes sobre o processo AVM podem ser encontrados na [documentacao de contribuicao do Azure Verified Modules](https://azure.github.io/Azure-Verified-Modules/contributing/terraform/testing/).
