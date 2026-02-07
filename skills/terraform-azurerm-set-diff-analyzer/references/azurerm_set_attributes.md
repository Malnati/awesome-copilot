# Referencia de Atributos Set-Type do AzureRM

Este documento explica a visao geral e a manutencao de `azurerm_set_attributes.json`.

> **Ultima Atualizacao**: January 28, 2026

## Visao Geral

`azurerm_set_attributes.json` e um arquivo de definicao para atributos tratados como Set-type no AzureRM Provider.
O script `analyze_plan.py` le esse JSON para identificar "false-positive diffs" em planos do Terraform.

### O que sao Set-Type Attributes?

O tipo Set do Terraform e uma colecao que **nao garante ordem**.
Portanto, ao adicionar ou remover elementos, elementos inalterados podem aparecer como "changed".
Isso e chamado de "false-positive diff".

## Estrutura do Arquivo JSON

### Formato Basico

```json
{
  "resources": {
    "azurerm_resource_type": {
      "attribute_name": "key_attribute"
    }
  }
}
```

- **key_attribute**: O atributo que identifica unicamente elementos do Set (ex.: `name`, `id`)
- **null**: Quando nao ha key attribute (comparar o elemento inteiro)

### Formato Aninhado

Quando um atributo Set contem outro atributo Set:

```json
{
  "rewrite_rule_set": {
    "_key": "name",
    "rewrite_rule": {
      "_key": "name",
      "condition": "variable",
      "request_header_configuration": "header_name"
    }
  }
}
```

- **`_key`**: O key attribute para elementos do Set nesse nivel
- **Outras chaves**: Definicoes para atributos Set aninhados

### Exemplo: azurerm_application_gateway

```json
"azurerm_application_gateway": {
  "backend_address_pool": "name",           // Simple Set (key is name)
  "rewrite_rule_set": {                     // Nested Set
    "_key": "name",
    "rewrite_rule": {
      "_key": "name",
      "condition": "variable"
    }
  }
}
```

## Manutencao

### Adicionar Novos Atributos

1. **Verifique a Documentacao Oficial**
   - Procure o recurso em [Terraform Registry](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)
   - Verifique se o atributo esta listado como "Set of ..."
   - Alguns recursos como `azurerm_application_gateway` tem atributos Set anotados explicitamente

2. **Verifique o Codigo-Fonte (mais confiavel)**
   - Procure o recurso em [AzureRM Provider GitHub](https://github.com/hashicorp/terraform-provider-azurerm)
   - Confirme `Type: pluginsdk.TypeSet` na definicao do schema
   - Identifique atributos dentro do `Schema` do Set que podem servir como `_key`

3. **Adicionar ao JSON**
   ```json
   "azurerm_new_resource": {
     "set_attribute": "key_attribute"
   }
   ```

4. **Teste**
   ```bash
   # Verify with an actual plan
   python3 scripts/analyze_plan.py your_plan.json
   ```

### Identificar Key Attributes

| Common Key Attribute | Usage |
|---------------------|-------|
| `name` | Named blocks (most common) |
| `id` | Resource ID reference |
| `location` | Geographic location |
| `address` | Network address |
| `host_name` | Hostname |
| `null` | When no key exists (compare entire element) |

## Tools Relacionadas

### analyze_plan.py

Analisa o plano JSON do Terraform para identificar false-positive diffs.

```bash
# Basic usage
terraform show -json plan.tfplan | python3 scripts/analyze_plan.py

# Read from file
python3 scripts/analyze_plan.py plan.json

# Use custom attribute file
python3 scripts/analyze_plan.py plan.json --attributes /path/to/custom.json
```

## Recursos Suportados

Consulte `azurerm_set_attributes.json` diretamente para recursos suportados atualmente:

```bash
# List resources
jq '.resources | keys' azurerm_set_attributes.json
```

Key resources:
- `azurerm_application_gateway` - Backend pools, listeners, rules, etc.
- `azurerm_firewall_policy_rule_collection_group` - Rule collections
- `azurerm_frontdoor` - Backend pools, routing
- `azurerm_network_security_group` - Security rules
- `azurerm_virtual_network_gateway` - IP configuration, VPN client configuration

## Notas

- O comportamento de atributos pode variar conforme a versao do Provider/API
- Novos recursos e atributos precisam ser adicionados conforme se tornem disponiveis
- Definir todos os niveis de estruturas profundamente aninhadas melhora a precisao
