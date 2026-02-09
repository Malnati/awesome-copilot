---
name: terraform-azurerm-set-diff-analyzer
description: Analise a saida JSON do terraform plan para o provider AzureRM para distinguir diffs falso-positivos (mudancas apenas de ordenacao em atributos do tipo Set) de mudancas reais de recurso. Use ao revisar saida do terraform plan para recursos Azure como Application Gateway, Load Balancer, Firewall, Front Door, NSG e outros com atributos Set que causam diffs espurios por mudancas internas de ordem.
license: MIT
---

# Analisador de Diff de Set do Terraform AzureRM

Uma skill para identificar "false-positive diffs" (diffs falso-positivos) em planos Terraform causados por atributos do tipo Set do AzureRM Provider e distingui-los de mudancas reais.

## Quando Usar

- `terraform plan` mostra muitas mudancas, mas voce so adicionou/removeu um elemento
- Application Gateway, Load Balancer, NSG etc. mostram "todos os elementos mudaram"
- Voce quer filtrar automaticamente diffs falso-positivos em CI/CD

## Contexto

O tipo Set do Terraform compara por posicao em vez de chave, entao ao adicionar ou remover elementos, todos parecem "mudados". Isso e um problema geral do Terraform, mas e especialmente notavel em recursos AzureRM que usam muitos atributos Set como Application Gateway, Load Balancer e NSG.

Esses "false-positive diffs" nao afetam de fato os recursos, mas dificultam a revisao da saida do terraform plan.

## Pre-requisitos

- Python 3.8+

Se Python nao estiver disponivel, instale via package manager (ex.: `apt install python3`, `brew install python3`) ou em [python.org](https://www.python.org/downloads/).

## Uso Basico

```bash
# 1. Generate plan JSON output
terraform plan -out=plan.tfplan
terraform show -json plan.tfplan > plan.json

# 2. Analyze
python scripts/analyze_plan.py plan.json
```

## Solucao de Problemas

- **`python: command not found`**: Use `python3` ou instale Python
- **`ModuleNotFoundError`**: O script usa apenas biblioteca padrao; garanta Python 3.8+

## Documentacao Detalhada

- [scripts/README.md](scripts/README.md) - Todas as opcoes, formatos de saida, exit codes, exemplos CI/CD
- [references/azurerm_set_attributes.md](references/azurerm_set_attributes.md) - Recursos e atributos suportados
