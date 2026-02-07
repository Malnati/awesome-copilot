# Script de Analise de Set Diff do Terraform AzureRM

Um script Python que analisa JSON de planos do Terraform e identifica "false-positive diffs" em atributos Set-type do AzureRM.

## Visao Geral

Atributos Set-type do AzureRM Provider (como `backend_address_pool`, `security_rule`, etc.) nao garantem ordem, entao ao adicionar ou remover elementos, todos os elementos aparecem como "changed". Este script distingue esses "false-positive diffs" de mudancas reais.

### Casos de Uso

- Como **Agent Skill** (recomendado)
- Como **CLI tool** para execucao manual
- Para analise automatizada em **CI/CD pipelines**

## Pre-requisitos

- Python 3.8 ou superior
- Sem pacotes adicionais (usa apenas a standard library)

## Uso

### Uso Basico

```bash
# Read from file
python analyze_plan.py plan.json

# Read from stdin
terraform show -json plan.tfplan | python analyze_plan.py
```

### Opcoes

| Opcao | Curto | Descricao | Padrao |
|--------|-------|-------------|---------|
| `--format` | `-f` | Formato de saida (markdown/json/summary) | markdown |
| `--exit-code` | `-e` | Retornar exit code com base em mudancas | false |
| `--quiet` | `-q` | Suprimir warnings | false |
| `--verbose` | `-v` | Mostrar warnings detalhados | false |
| `--ignore-case` | - | Comparar valores sem diferenciar maiusculas/minusculas | false |
| `--attributes` | - | Caminho para arquivo de definicao de atributos customizado | (built-in) |
| `--include` | - | Filtrar recursos para analisar (pode especificar multiplos) | (all) |
| `--exclude` | - | Filtrar recursos para excluir (pode especificar multiplos) | (none) |

### Exit Codes (com `--exit-code`)

| Codigo | Significado |
|------|---------|
| 0 | Sem mudancas, ou apenas mudancas de ordem |
| 1 | Mudancas reais em atributos Set |
| 2 | Substituicao de recurso (delete + create) |
| 3 | Erro |

## Formatos de Saida

### Markdown (default)

Formato legivel para comentarios de PR e relatorios.

```bash
python analyze_plan.py plan.json --format markdown
```

### JSON

Dados estruturados para processamento programatico.

```bash
python analyze_plan.py plan.json --format json
```

Exemplo de saida:
```json
{
  "summary": {
    "order_only_count": 3,
    "actual_set_changes_count": 1,
    "replace_count": 0
  },
  "has_real_changes": true,
  "resources": [...],
  "warnings": []
}
```

### Resumo

Resumo em uma linha para logs de CI/CD.

```bash
python analyze_plan.py plan.json --format summary
```

Exemplo de saida:
```
🟢 3 order-only | 🟡 1 set changes
```

## Uso em Pipeline CI/CD

### GitHub Actions

```yaml
name: Terraform Plan Analysis

on:
  pull_request:
    paths:
      - '**.tf'

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        
      - name: Terraform Init & Plan
        run: |
          terraform init
          terraform plan -out=plan.tfplan
          terraform show -json plan.tfplan > plan.json
          
      - name: Analyze Set Diff
        run: |
          python path/to/analyze_plan.py plan.json --format markdown > analysis.md
          
      - name: Comment PR
        uses: marocchino/sticky-pull-request-comment@v2
        with:
          path: analysis.md
```

### GitHub Actions (Gate com Exit Code)

```yaml
      - name: Analyze and Gate
        run: |
          python path/to/analyze_plan.py plan.json --exit-code --format summary
        # Fail on exit code 2 (resource replacement)
        continue-on-error: false
```

### Azure Pipelines

```yaml
- task: TerraformCLI@0
  inputs:
    command: 'plan'
    commandOptions: '-out=plan.tfplan'

- script: |
    terraform show -json plan.tfplan > plan.json
    python scripts/analyze_plan.py plan.json --format markdown > $(Build.ArtifactStagingDirectory)/analysis.md
  displayName: 'Analyze Plan'

- task: PublishBuildArtifacts@1
  inputs:
    pathToPublish: '$(Build.ArtifactStagingDirectory)/analysis.md'
    artifactName: 'plan-analysis'
```

### Exemplos de Filtragem

Analisar apenas recursos especificos:
```bash
python analyze_plan.py plan.json --include application_gateway --include load_balancer
```

Excluir recursos especificos:
```bash
python analyze_plan.py plan.json --exclude virtual_network
```

## Interpretando Resultados

| Categoria | Significado | Acao Recomendada |
|----------|-------------|-----------------|
| 🟢 Order-only | False-positive diff, sem mudanca real | Pode ignorar |
| 🟡 Actual change | Elemento Set adicionado/removido/modificado | Revise o conteudo, geralmente update in-place |
| 🔴 Resource replacement | delete + create | Verifique impacto de downtime |

## Definicoes Customizadas de Atributos

Por default, usa `references/azurerm_set_attributes.json`, mas voce pode especificar um arquivo de definicao customizado:

```bash
python analyze_plan.py plan.json --attributes /path/to/custom_attributes.json
```

Veja `references/azurerm_set_attributes.md` para o formato do arquivo de definicao.

## Limitacoes

- Apenas recursos AzureRM (`azurerm_*`) sao suportados
- Alguns recursos/atributos podem nao ser suportados
- Comparacoes podem ser incompletas para atributos com `after_unknown` (valores definidos apos apply)
- Comparacoes podem ser incompletas para atributos sensiveis (mascarados)
