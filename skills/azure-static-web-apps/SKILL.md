---
name: azure-static-web-apps
description: Ajuda a criar, configurar e fazer deploy de Azure Static Web Apps usando o SWA CLI. Use ao fazer deploy de sites estaticos no Azure, configurar desenvolvimento local do SWA, configurar staticwebapp.config.json, adicionar APIs Azure Functions ao SWA ou configurar CI/CD do GitHub Actions para Static Web Apps.
---

## Visao Geral

Azure Static Web Apps (SWA) hospeda frontends estaticos com backends opcionais de API serverless. O SWA CLI (`swa`) fornece emulacao de desenvolvimento local e capacidades de deploy.

**Principais features:**
- Emulador local com proxy de API e simulacao de auth
- Auto-deteccao e configuracao de frameworks
- Deploy direto para Azure
- Suporte a conexoes de banco de dados

**Arquivos de configuracao:**
- `swa-cli.config.json` - Configuracoes do CLI, **criado por `swa init`** (nunca crie manualmente)
- `staticwebapp.config.json` - Configuracao de runtime (rotas, auth, headers, runtime de API) - pode ser criado manualmente

## Instrucoes Gerais

### Instalacao

```bash
npm install -D @azure/static-web-apps-cli
```

Verifique: `npx swa --version`

### Workflow de Inicio Rapido

**IMPORTANTE: Sempre use `swa init` para criar arquivos de configuracao. Nunca crie `swa-cli.config.json` manualmente.**

1. `swa init` - **Primeiro passo obrigatorio** - auto-detecta framework e cria `swa-cli.config.json`
2. `swa start` - Execute o emulador local em `http://localhost:4280`
3. `swa login` - Autentique com o Azure
4. `swa deploy` - Faca deploy no Azure

### Arquivos de Configuracao

**swa-cli.config.json** - Criado por `swa init`, nao criar manualmente:
- Execute `swa init` para setup interativo com deteccao de framework
- Execute `swa init --yes` para aceitar defaults auto-detectados
- Edite o arquivo gerado apenas para customizar settings apos a inicializacao

Exemplo de config gerado (apenas referencia):
```json
{
  "$schema": "https://aka.ms/azure/static-web-apps-cli/schema",
  "configurations": {
    "app": {
      "appLocation": ".",
      "apiLocation": "api",
      "outputLocation": "dist",
      "appBuildCommand": "npm run build",
      "run": "npm run dev",
      "appDevserverUrl": "http://localhost:3000"
    }
  }
}
```

**staticwebapp.config.json** (na pasta de origem do app ou output) - Este arquivo PODE ser criado manualmente para configuracao de runtime:
```json
{
  "navigationFallback": {
    "rewrite": "/index.html",
    "exclude": ["/images/*", "/css/*"]
  },
  "routes": [
    { "route": "/api/*", "allowedRoles": ["authenticated"] }
  ],
  "platform": {
    "apiRuntime": "node:20"
  }
}
```

## Referencia de Linha de Comando

### swa login

Autentique com o Azure para deploy.

```bash
swa login                              # Interactive login
swa login --subscription-id <id>       # Specific subscription
swa login --clear-credentials          # Clear cached credentials
```

**Flags:** `--subscription-id, -S` | `--resource-group, -R` | `--tenant-id, -T` | `--client-id, -C` | `--client-secret, -CS` | `--app-name, -n`

### swa init

Configure um novo projeto SWA com base em um frontend existente e (opcionalmente) uma API. Detecta frameworks automaticamente.

```bash
swa init                    # Interactive setup
swa init --yes              # Accept defaults
```

### swa build

Build do frontend e/ou API.

```bash
swa build                   # Build using config
swa build --auto            # Auto-detect and build
swa build myApp             # Build specific configuration
```

**Flags:** `--app-location, -a` | `--api-location, -i` | `--output-location, -O` | `--app-build-command, -A` | `--api-build-command, -I`

### swa start

Inicie o emulador de desenvolvimento local.

```bash
swa start                                    # Serve from outputLocation
swa start ./dist                             # Serve specific folder
swa start http://localhost:3000              # Use a dev server
swa start http://localhost:3000 --api-location api
swa start http://localhost:3000 --run "npm run dev" --api-location api
```

**Flags:** `--api-location, -i` | `--output-location, -O` | `--host, -H` | `--port, -P` | `--run, -r` | `--verbose, -v`

### swa deploy

Faca deploy para o Azure.

```bash
swa deploy                                   # Deploy using config
swa deploy ./dist                            # Deploy specific folder
swa deploy ./dist --env production           # Deploy with environment
swa deploy ./dist --app-name my-swa          # Deploy to specific app
```

**Flags:** `--app-name, -n` | `--env, -e` | `--resource-group, -g` | `--subscription-id, -S` | `--api-location, -i` | `--verbose, -v`

## CI/CD com GitHub Actions

Use o workflow do GitHub Actions gerado pelo Azure Static Web Apps. O deploy e automatico com push para a branch configurada.

### Dicas

- Use `swa init` para configurar o projeto e gerar `swa-cli.config.json`
- Nao comite credenciais no repositorio
- Use `staticwebapp.config.json` para configurar rotas, auth e headers
