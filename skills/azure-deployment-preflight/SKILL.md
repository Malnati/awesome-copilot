---
name: azure-deployment-preflight
description: 'Executa validacao preflight abrangente de deployments Bicep no Azure, incluindo validacao de sintaxe de template, analise what-if e checagens de permissao. Use esta skill antes de qualquer deployment no Azure para prever mudancas, identificar problemas potenciais e garantir que o deployment sera bem-sucedido. Ative quando usuarios mencionarem deploy no Azure, validar arquivos Bicep, checar permissoes de deployment, prever mudancas de infraestrutura, rodar what-if ou preparar azd provision.'
---

# Validacao Preflight de Deployment no Azure

Esta skill valida deployments Bicep antes da execucao, suportando workflows do Azure CLI (`az`) e Azure Developer CLI (`azd`).

## Quando Usar Esta Skill

- Antes de fazer deploy de infraestrutura no Azure
- Ao preparar ou revisar arquivos Bicep
- Para prever quais mudancas um deployment fara
- Para verificar se as permissoes sao suficientes para o deployment
- Antes de rodar `azd up`, `azd provision` ou comandos `az deployment`

## Processo de Validacao

Siga estas etapas em ordem. Continue para a proxima etapa mesmo se a anterior falhar — capture todos os issues no relatorio final.

### Etapa 1: Detectar Tipo de Projeto

Determine o workflow de deployment checando indicadores do projeto:

1. **Verificar projeto azd**: Procure `azure.yaml` na raiz do projeto
   - Se encontrado → Use **workflow azd**
   - Se nao encontrado → Use **workflow az CLI**

2. **Localizar arquivos Bicep**: Encontre todos os arquivos `.bicep` para validar
   - Para projetos azd: verifique primeiro `infra/`, depois a raiz do projeto
   - Para standalone: use o arquivo especificado pelo usuario ou procure locais comuns (`infra/`, `deploy/`, raiz do projeto)

3. **Auto-detectar arquivos de parametros**: Para cada arquivo Bicep, procure arquivos de parametros correspondentes:
   - `<filename>.bicepparam` (parametros Bicep - preferido)
   - `<filename>.parameters.json` (parametros JSON)
   - `parameters.json` ou `parameters/<env>.json` no mesmo diretorio

### Etapa 2: Validar Sintaxe Bicep

Execute o Bicep CLI para checar a sintaxe do template antes de tentar validacao de deployment:

```bash
bicep build <bicep-file> --stdout
```

**O que capturar:**
- Erros de sintaxe com numeros de linha/coluna
- Mensagens de warning
- Status de sucesso/falha do build

**Se o Bicep CLI nao estiver instalado:**
- Registre o problema no relatorio
- Continue para a Etapa 3 (o Azure validara sintaxe durante o what-if)

### Etapa 3: Rodar Validacao Preflight

Escolha a validacao apropriada com base no tipo de projeto detectado na Etapa 1.

#### Para Projetos azd (azure.yaml existe)

Use `azd provision --preview` para validar o deployment:

```bash
azd provision --preview
```

Se um ambiente estiver especificado ou houver multiplos ambientes:
```bash
azd provision --preview --environment <env-name>
```

#### Para Bicep Standalone (sem azure.yaml)

Determine o escopo do deployment a partir da declaracao `targetScope` do arquivo Bicep:

| Escopo de Destino (Target Scope) | Comando |
|----------------------------------|---------|
| `resourceGroup` (default) | `az deployment group what-if` |
| `subscription` | `az deployment sub what-if` |
| `managementGroup` | `az deployment mg what-if` |
| `tenant` | `az deployment tenant what-if` |

**Execute primeiro com nivel de validacao Provider:**

```bash
# Resource Group scope (most common)
az deployment group what-if \
  --resource-group <rg-name> \
  --template-file <bicep-file> \
  --parameters <param-file> \
  --validation-level Provider

# Subscription scope
az deployment sub what-if \
  --location <location> \
  --template-file <bicep-file> \
  --parameters <param-file> \
  --validation-level Provider

# Management Group scope
az deployment mg what-if \
  --location <location> \
  --management-group-id <mg-id> \
  --template-file <bicep-file> \
  --parameters <param-file> \
  --validation-level Provider

# Tenant scope
az deployment tenant what-if \
  --location <location> \
  --template-file <bicep-file> \
  --parameters <param-file> \
  --validation-level Provider
```

**Estrategia de Fallback:**

Se `--validation-level Provider` falhar com erros de permissao (RBAC), tente novamente com `ProviderNoRbac`:

```bash
az deployment group what-if \
  --resource-group <rg-name> \
  --template-file <bicep-file> \
  --validation-level ProviderNoRbac
```

Registre o fallback no relatorio — o usuario pode nao ter permissoes completas de deployment.

### Etapa 4: Capturar Resultados do What-If

Parseie a saida do what-if para categorizar mudancas de recursos:

| Tipo de Mudanca | Simbolo | Significado |
|-----------------|---------|-------------|
| Criar | `+` | Recurso novo sera criado |
| Deletar | `-` | Recurso sera deletado |
| Modify | `~` | Propriedades do recurso mudarao |
| NoChange | `=` | Recurso sem mudancas |
| Ignore | `*` | Recurso nao analisado (limites atingidos) |
| Deploy | `!` | Recurso sera implantado (mudancas desconhecidas) |

Para recursos modificados, capture as mudancas especificas de propriedades.

### Etapa 5: Gerar Relatorio

Crie um arquivo de relatorio Markdown na **raiz do projeto** chamado:
- `preflight-report.md`

Use a estrutura do template em [references/REPORT-TEMPLATE.md](references/REPORT-TEMPLATE.md).

**Secoes do relatorio:**
1. **Resumo** - Status geral, timestamp, arquivos validados, escopo de destino (target scope)
2. **Tools Executadas** - Comandos rodados, versoes, niveis de validacao usados
3. **Problemas (Issues)** - Todos os erros e warnings com severidade e remediacao
4. **Resultados do What-If** - Recursos a criar/modificar/deletar/sem mudanca
5. **Recomendacoes** - Proximos passos acionaveis

## Informacoes Necessarias

Antes de rodar a validacao, colete:

| Informacao | Necessaria para | Como obter |
|-------------|----------------|------------|
| Resource Group | `az deployment group` | Pergunte ao usuario ou verifique config `.azure/` existente |
| Subscription | Todos os deployments | `az account show` ou pergunte ao usuario |
| Location | Escopo Sub/MG/Tenant | Pergunte ao usuario ou use default da configuracao |
| Environment | Projetos azd | `azd env list` ou pergunte ao usuario |

Se informacao necessaria estiver faltando, pergunte ao usuario antes de prosseguir.

## Tratamento de Erros

Veja [references/ERROR-HANDLING.md](references/ERROR-HANDLING.md) para orientacao detalhada de tratamento de erros.

**Principio chave:** Continue a validacao mesmo quando erros ocorrerem. Capture todos os issues no relatorio final.

| Tipo de Erro | Acao |
|------------|--------|
| Nao logado | Registre no relatorio, sugira `az login` ou `azd auth login` |
| Permissao negada | Faça fallback para `ProviderNoRbac`, registre no relatorio |
| Erro de sintaxe Bicep | Inclua todos os erros, continue para outros arquivos |
| Tool nao instalada | Registre no relatorio, pule essa etapa de validacao |
| Resource group nao encontrado | Registre no relatorio, sugira cria-lo |

## Requisitos de Ferramentas

Esta skill usa as seguintes tools:

- **Azure CLI** (`az`) - Versao 2.76.0+ recomendada para `--validation-level`
- **Azure Developer CLI** (`azd`) - Para projetos com `azure.yaml`
- **Bicep CLI** (`bicep`) - Para validacao de sintaxe
- **Azure MCP Tools** - Para consultas de documentacao e boas praticas

Verifique disponibilidade das tools antes de iniciar:
```bash
az --version
azd version
bicep --version
```
