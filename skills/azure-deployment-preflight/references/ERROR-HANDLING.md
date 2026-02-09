# Guia de Tratamento de Erros

Este documento referencia erros comuns durante a validacao preflight e como trata-los.

## Principio Central

**Continue em caso de falha.** Registre todos os problemas no relatorio final em vez de parar no primeiro erro. Isso oferece aos usuarios uma visao completa do que precisa ser corrigido.

---

## Erros de Autenticacao

### Nao Autenticado (Azure CLI)

**Deteccao:**
```
ERROR: Please run 'az login' to setup account.
ERROR: AADSTS700082: The refresh token has expired
```

**Exit Codes:** Nao zero

**Tratamento:**
1. Anote o erro no relatorio
2. Inclua passos de remediacao
3. Pule os comandos do Azure CLI restantes
4. Continue com outras etapas de validacao se possivel

**Entrada no Relatorio:**
```markdown
#### ❌ Azure CLI Authentication Required

- **Severity:** Error
- **Source:** az cli
- **Message:** Nao autenticado to Azure CLI
- **Remediation:** Execute `az login` para autenticar e depois rode novamente a validacao preflight
- **Documentacao:** https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli
```

### Nao Autenticado (azd)

**Deteccao:**
```
ERROR: not logged in, run `azd auth login` to login
```

**Tratamento:**
1. Anote o erro no relatorio
2. Pule comandos azd
3. Sugira `azd auth login`

**Entrada no Relatorio:**
```markdown
#### ❌ Azure Developer CLI Authentication Required

- **Severity:** Error
- **Source:** azd
- **Message:** Nao autenticado to Azure Developer CLI
- **Remediation:** Execute `azd auth login` para autenticar e depois rode novamente a validacao preflight
```

### Token Expirado

**Deteccao:**
```
AADSTS700024: Client assertion is not within its valid time range
AADSTS50173: The provided grant has expired
```

**Tratamento:**
1. Anote o erro
2. Sugira reautenticacao
3. Pule operacoes do Azure

---

## Erros de Permissao

### Permissoes RBAC Insuficientes

**Deteccao:**
```
AuthorizationFailed: The client '...' with object id '...' does not have authorization 
to perform action '...' over scope '...'
```

**Tratamento:**
1. **Primeira tentativa:** Repetir com `--validation-level ProviderNoRbac`
2. Anote a limitacao de permissao no relatorio
3. Se ProviderNoRbac falhar, reporte a permissao ausente especifica

**Entrada no Relatorio:**
```markdown
#### ⚠️ Limited Permission Validation

- **Severity:** Warning
- **Source:** what-if
- **Message:** Validacao RBAC completa falhou; usando validacao somente leitura
- **Detail:** Permissao ausente: `Microsoft.Resources/deployments/write` on scope `/subscriptions/xxx`
- **Recommendation:** Request Contributor role on the target resource group, or verify deployment permissions with your administrator
```

### Resource Group Nao Encontrado

**Deteccao:**
```
ResourceGroupNotFound: Resource group 'xxx' could not be found.
```

**Tratamento:**
1. Anote no relatorio
2. Sugira criar o resource group
3. Pule what-if para esse escopo

**Entrada no Relatorio:**
```markdown
#### ❌ Resource Group Does Not Exist

- **Severity:** Error
- **Source:** what-if
- **Message:** Resource group 'my-rg' does not exist
- **Remediation:** Crie o resource group antes do deploy:
  ```bash
  az group create --name my-rg --location eastus
  ```
```

### Acesso a Subscription Negado

**Deteccao:**
```
SubscriptionNotFound: The subscription 'xxx' could not be found.
InvalidSubscriptionId: Subscription '...' is not valid
```

**Tratamento:**
1. Anote no relatorio
2. Sugira verificar o subscription ID
3. Liste subscriptions disponiveis

---

## Erros de Sintaxe Bicep

### Erros de Compilacao

**Deteccao:**
```
/path/main.bicep(22,51) : Error BCP064: Found unexpected tokens
/path/main.bicep(10,5) : Error BCP018: Expected the "=" character at this location
```

**Tratamento:**
1. Parse o output de erro para obter linha/coluna
2. Inclua todos os erros no relatorio (nao pare no primeiro)
3. Continue para what-if (pode dar contexto adicional)

**Entrada no Relatorio:**
```markdown
#### ❌ Bicep Syntax Error

- **Severity:** Error
- **Source:** bicep build
- **Location:** `main.bicep:22:51`
- **Code:** BCP064
- **Message:** Found unexpected tokens in interpolated expression
- **Remediation:** Check the string interpolation syntax at line 22
- **Documentacao:** https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/diagnostics/bcp064
```

### Modulo Nao Encontrado

**Deteccao:**
```
Error BCP091: An error occurred reading file. Could not find file '...'
Error BCP190: The module is not valid
```

**Tratamento:**
1. Anote o modulo ausente
2. Verifique se `bicep restore` e necessario
3. Verifique o caminho do modulo

### Problemas no Arquivo de Parametros

**Deteccao:**
```
Error BCP032: The value must be a compile-time constant
Error BCP035: The specified object is missing required properties
```

**Tratamento:**
1. Anote problemas de parametro
2. Indique quais parametros estao problematicos
3. Sugira correcoes

---

## Ferramenta Nao Instalada

### Azure CLI Nao Encontrado

**Deteccao:**
```
'az' is not recognized as an internal or external command
az: command not found
```

**Tratamento:**
1. Anote no relatorio
2. Forneca instrucoes de instalacao.
  - Se disponivel, use a ferramenta Azure MCP `extension_cli_install` para obter instrucoes de instalacao.
  - Caso contrario, procure instrucoes em https://learn.microsoft.com/en-us/cli/azure/install-azure-cli.
3. Pule comandos az

**Entrada no Relatorio:**
```markdown
#### ⏭️ Azure CLI Not Installed

- **Severity:** Warning
- **Source:** environment
- **Message:** Azure CLI (az) is not installed or not in PATH
- **Remediation:** Instale o Azure CLI <ADD INSTALLATION INSTRUCTIONS HERE>
- **Impact:** What-if validation using az commands was skipped
```

### Bicep CLI Nao Encontrado

**Deteccao:**
```
'bicep' is not recognized as an internal or external command
bicep: command not found
```

**Tratamento:**
1. Anote no relatorio
2. Azure CLI pode ter Bicep embutido - tente `az bicep build`
3. Forneca link de instalacao

**Entrada no Relatorio:**
```markdown
#### ⏭️ Bicep CLI Not Installed

- **Severity:** Warning
- **Source:** environment
- **Message:** Bicep CLI is not installed
- **Remediation:** Instale o Bicep CLI: https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/install
- **Impact:** A validacao de sintaxe foi pulada; o Azure vai validar durante o what-if
```

### Azure Developer CLI Nao Encontrado

**Deteccao:**
```
'azd' is not recognized as an internal or external command
azd: command not found
```

**Tratamento:**
1. Se `azure.yaml` existir, esta ferramenta e obrigatoria
2. Volte para comandos az CLI se possivel
3. Anote no relatorio

---

## Erros Especificos do What-If

### Limites de Nested Template

**Deteccao:**
```
The deployment exceeded the nested template limit of 500
```

**Tratamento:**
1. Anote como warning (nao erro)
2. Explique que recursos afetados aparecem como "Ignore"
3. Sugira revisao manual

### Template Link Nao Suportado

**Deteccao:**
```
templateLink references in nested deployments won't be visible in what-if
```

**Tratamento:**
1. Anote como warning
2. Explique a limitacao
3. Recursos serao verificados durante o deploy real

### Expressoes Nao Avaliadas

**Deteccao:** Propriedades exibindo nomes de funcao como `[utcNow()]` em vez de valores

**Tratamento:**
1. Anote como informativo
2. Explique que sao avaliadas no momento do deploy
3. Nao e um erro

---

## Erros de Rede

### Timeout

**Deteccao:**
```
Connection timed out
Request timed out
```

**Tratamento:**
1. Sugira tentar novamente
2. Verifique conectividade de rede
3. Pode indicar problemas de servico Azure

### Erros de SSL/TLS

**Deteccao:**
```
SSL: CERTIFICATE_VERIFY_FAILED
unable to get local issuer certificate
```

**Tratamento:**
1. Anote no relatorio
2. Pode indicar proxy ou firewall corporativo
3. Sugira verificar configuracoes SSL

---

## Estrategia de Fallback

Quando a validacao primaria falhar, tente fallbacks na ordem:

```
Provider (full RBAC validation)
    ↓ fails with permission error
ProviderNoRbac (validation without write permission check)
    ↓ fails
Template (static syntax only)
    ↓ fails
Relate todas as falhas e pule a analise what-if
```

**Sempre gere o relatorio**, mesmo que todas as etapas de validacao falhem.

---

## Agregacao de Relatorio de Erros

Quando houver multiplos erros, agregue-os de forma logica:

1. **Agrupar por fonte** (bicep, what-if, permissions)
2. **Ordenar por severidade** (erros antes de warnings)
3. **Deduplicar** erros similares
4. **Fornecer contagem resumida** no topo

Exemplo:
```markdown
## Issues

Found **3 errors** and **2 warnings**

### Errors (3)

1. [Bicep Syntax Error - main.bicep:22:51](#error-1)
2. [Bicep Syntax Error - main.bicep:45:10](#error-2)
3. [Resource Group Not Found](#error-3)

### Warnings (2)

1. [Limited Permission Validation](#warning-1)
2. [Nested Template Limit Reached](#warning-2)
```

---

## Referencia de Exit Codes

| Ferramenta | Exit Code | Significado |
|------|-----------|---------|
| az | 0 | Sucesso |
| az | 1 | Erro geral |
| az | 2 | Comando nao encontrado |
| az | 3 | Argumento obrigatorio ausente |
| azd | 0 | Sucesso |
| azd | 1 | Erro |
| bicep | 0 | Build concluido |
| bicep | 1 | Build falhou (erros) |
| bicep | 2 | Build concluido com warnings |
