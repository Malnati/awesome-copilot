# Referencia de Comandos de Validacao

Este documento referencia todos os comandos usados na validacao preflight de deploy Azure.

## Azure Developer CLI (azd)

### azd provision --preview

Pré-visualiza mudancas de infraestrutura para projetos azd sem realizar deploy.

```bash
azd provision --preview [options]
```

**Opcoes:**
| Opcao | Descricao |
|--------|-------------|
| `--environment`, `-e` | Name of the environment to use |
| `--no-prompt` | Accept defaults without prompting |
| `--debug` | Habilitar debug logging |
| `--cwd` | Set working directory |

**Exemplos:**

```bash
# Preview with default environment
azd provision --preview

# Preview specific environment
azd provision --preview --environment dev

# Preview without prompts (CI/CD)
azd provision --preview --no-prompt
```

**Saida:** Shows resources that will be created, modified, or deleted.

### azd auth login

Autentica no Azure para operacoes azd.

```bash
azd auth login [options]
```

**Opcoes:**
| Opcao | Descricao |
|--------|-------------|
| `--check-status` | Check login status without logging in |
| `--use-device-code` | Use device code flow |
| `--tenant-id` | Specify tenant |
| `--client-id` | Service principal client ID |

### azd env list

Lista ambientes disponiveis.

```bash
azd env list
```

---

## Azure CLI (az)

### az deployment group what-if

Preview de mudancas para deploys em resource group.

```bash
az deployment group what-if \
  --resource-group <rg-name> \
  --template-file <bicep-file> \
  [options]
```

**Parametros Obrigatorios:**
| Parametro | Descricao |
|-----------|-------------|
| `--resource-group`, `-g` | Target resource group name |
| `--template-file`, `-f` | Path to Bicep file |

**Parametros Opcionais:**
| Parametro | Descricao |
|-----------|-------------|
| `--parameters`, `-p` | Parameter file or inline values |
| `--validation-level` | `Provider` (default), `ProviderNoRbac`, or `Template` |
| `--result-format` | `FullResourcePayloads` (default) or `ResourceIdOnly` |
| `--no-pretty-print` | Saida JSON bruta para parsing |
| `--name`, `-n` | Deployment name |
| `--exclude-change-types` | Exclude specific change types from output |

**Niveis de Validacao:**
| Nivel | Descricao | Caso de Uso |
|-------|-------------|----------|
| `Provider` | Validacao completa com checagens RBAC | Padrao, mais completa |
| `ProviderNoRbac` | Full validation, read permissions only | When lacking deploy permissions |
| `Template` | Validacao estatica de sintaxe apenas | Checagem rapida de sintaxe |

**Exemplos:**

```bash
# Basic what-if
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep

# With parameters and full validation
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep \
  --parameters main.bicepparam \
  --validation-level Provider

# Fallback without RBAC checks
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep \
  --validation-level ProviderNoRbac

# JSON output for parsing
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep \
  --no-pretty-print
```

### az deployment sub what-if

Preview de mudancas para deploys em nivel de subscription.

```bash
az deployment sub what-if \
  --location <location> \
  --template-file <bicep-file> \
  [options]
```

**Parametros Obrigatorios:**
| Parametro | Descricao |
|-----------|-------------|
| `--location`, `-l` | Location for deployment metadata |
| `--template-file`, `-f` | Path to Bicep file |

**Exemplos:**

```bash
az deployment sub what-if \
  --location eastus \
  --template-file main.bicep \
  --parameters main.bicepparam \
  --validation-level Provider
```

### az deployment mg what-if

Preview de mudancas para deploys em management group.

```bash
az deployment mg what-if \
  --location <location> \
  --management-group-id <mg-id> \
  --template-file <bicep-file> \
  [options]
```

**Parametros Obrigatorios:**
| Parametro | Descricao |
|-----------|-------------|
| `--location`, `-l` | Location for deployment metadata |
| `--management-group-id`, `-m` | Target management group ID |
| `--template-file`, `-f` | Path to Bicep file |

### az deployment tenant what-if

Preview de mudancas para deploys em nivel de tenant.

```bash
az deployment tenant what-if \
  --location <location> \
  --template-file <bicep-file> \
  [options]
```

**Parametros Obrigatorios:**
| Parametro | Descricao |
|-----------|-------------|
| `--location`, `-l` | Location for deployment metadata |
| `--template-file`, `-f` | Path to Bicep file |

### az login

Autentica no Azure CLI.

```bash
az login [options]
```

**Opcoes:**
| Opcao | Descricao |
|--------|-------------|
| `--tenant`, `-t` | Tenant ID or domain |
| `--use-device-code` | Use device code flow |
| `--service-principal` | Login as service principal |

### az account show

Exibe o contexto atual da subscription.

```bash
az account show
```

### az group exists

Verifica se o resource group existe.

```bash
az group exists --name <rg-name>
```

---

## Bicep CLI

### bicep build

Compila Bicep para ARM JSON e valida sintaxe.

```bash
bicep build <bicep-file> [options]
```

**Opcoes:**
| Opcao | Descricao |
|--------|-------------|
| `--stdout` | Saida para stdout em vez de arquivo |
| `--outdir` | Diretorio de saida |
| `--outfile` | Caminho do arquivo de saida |
| `--no-restore` | Skip module restore |

**Exemplos:**

```bash
# Validate syntax (output to stdout, no file created)
bicep build main.bicep --stdout > /dev/null

# Build to specific directory
bicep build main.bicep --outdir ./build

# Validate multiple files
for f in *.bicep; do bicep build "$f" --stdout; done
```

**Formato de Saida de Erro:**
```
/path/to/file.bicep(22,51) : Error BCP064: Found unexpected tokens in interpolated expression.
/path/to/file.bicep(22,51) : Error BCP004: The string at this location is not terminated.
```

Format: `<file>(<line>,<column>) : <severity> <code>: <message>`

### bicep --version

Verifica a versao do Bicep CLI.

```bash
bicep --version
```

---

## Deteccao de Arquivo de Parametros

### Bicep Parameters (.bicepparam)

Arquivos modernos de parametros Bicep (recomendado):

```bicep
using './main.bicep'

param location = 'eastus'
param environment = 'dev'
param tags = {
  environment: 'dev'
  project: 'myapp'
}
```

**Padrao de deteccao:** `<template-name>.bicepparam`

### JSON Parameters (.parameters.json)

Arquivos tradicionais de parametros ARM:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": { "value": "eastus" },
    "environment": { "value": "dev" }
  }
}
```

**Padroes de deteccao:**
- `<template-name>.parameters.json`
- `parameters.json`
- `parameters/<env>.json`

### Usar Parametros com Comandos

```bash
# Bicep parameters file
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep \
  --parameters main.bicepparam

# JSON parameters file
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep \
  --parameters @parameters.json

# Inline parameter overrides
az deployment group what-if \
  --resource-group my-rg \
  --template-file main.bicep \
  --parameters main.bicepparam \
  --parameters location=westus
```

---

## Determinar Escopo de Deploy

Verifique a declaracao `targetScope` do arquivo Bicep:

```bicep
// Resource Group (default if not specified)
targetScope = 'resourceGroup'

// Subscription
targetScope = 'subscription'

// Management Group
targetScope = 'managementGroup'

// Tenant
targetScope = 'tenant'
```

**Mapeamento de Escopo para Comando:**

| targetScope | Comando | Parametros Obrigatorios |
|-------------|---------|---------------------|
| `resourceGroup` | `az deployment group what-if` | `--resource-group` |
| `subscription` | `az deployment sub what-if` | `--location` |
| `managementGroup` | `az deployment mg what-if` | `--location`, `--management-group-id` |
| `tenant` | `az deployment tenant what-if` | `--location` |

---

## Requisitos de Versao

| Ferramenta | Versao Minima | Versao Recomendada | Recursos Principais |
|------|-----------------|---------------------|--------------|
| Azure CLI | 2.14.0 | 2.76.0+ | `--validation-level` switch |
| Azure Developer CLI | 1.0.0 | Latest | `--preview` flag |
| Bicep CLI | 0.4.0 | Latest | Best error messages |

**Verificar versoes:**
```bash
az --version
azd version
bicep --version
```