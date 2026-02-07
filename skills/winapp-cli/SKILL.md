---
name: winapp-cli
description: 'CLI de desenvolvimento de apps Windows (winapp) para build, empacotamento e deploy de aplicacoes Windows. Use quando pedirem para inicializar projetos Windows, criar pacotes MSIX, gerar AppxManifest.xml, gerenciar certificados de desenvolvimento, adicionar package identity para debug, assinar pacotes ou acessar ferramentas de build do Windows SDK. Suporta .NET, C++, Electron, Rust, Tauri e frameworks multiplataforma com alvo Windows.'
---

# Windows App Development CLI

A Windows App Development CLI (`winapp`) e uma interface de linha de comando para gerenciar Windows SDKs, empacotamento MSIX, geracao de app identity, manifests, certificados e uso de ferramentas de build com qualquer framework. Ela preenche a lacuna entre desenvolvimento multiplataforma e capacidades nativas do Windows.

## Quando Usar Esta Skill

Use esta skill quando precisar:

- Inicializar um projeto Windows com setup de SDK, manifests e certificados
- Criar pacotes MSIX a partir de diretorios de aplicacao
- Gerar ou gerenciar arquivos AppxManifest.xml
- Criar e instalar certificados de desenvolvimento para assinatura
- Adicionar package identity para depurar APIs Windows
- Assinar pacotes MSIX ou executaveis
- Acessar ferramentas de build do Windows SDK a partir de qualquer framework
- Buildar apps Windows usando frameworks multiplataforma (Electron, Rust, Tauri, Qt)
- Configurar pipelines CI/CD para deploy de apps Windows
- Acessar APIs Windows que exigem package identity (notifications, Windows AI, shell integration)

## Pre-requisitos

- Windows 10 ou superior
- winapp CLI instalada por um destes metodos:
  - **WinGet**: `winget install Microsoft.WinAppCli --source winget`
  - **NPM** (para Electron): `npm install @microsoft/winappcli --save-dev`
  - **GitHub Actions/Azure DevOps**: Use a action [setup-WinAppCli](https://github.com/microsoft/setup-WinAppCli)
  - **Manual**: Download em [GitHub Releases](https://github.com/microsoft/WinAppCli/releases/latest)

## Capacidades Principais

### 1. Inicializacao de Projeto (`winapp init`)

Inicializa um diretorio com ativos necessarios (manifest, certificados, bibliotecas) para construir um app Windows moderno. Suporta modos de instalacao de SDK: `stable`, `preview`, `experimental` ou `none`.

### 2. Empacotamento MSIX (`winapp pack`)

Cria pacotes MSIX a partir de diretorios preparados com assinatura opcional, geracao de certificado e bundling de deploy self-contained.

### 3. Package Identity para Debug (`winapp create-debug-identity`)

Adiciona package identity temporaria a executaveis para depurar APIs Windows que exigem identity (notifications, Windows AI, shell integration) sem empacotamento completo.

### 4. Gerenciamento de Manifest (`winapp manifest`)

Gera arquivos AppxManifest.xml e atualiza assets de imagem a partir de imagens fonte, criando automaticamente todos os tamanhos e proporcoes exigidos.

### 5. Gerenciamento de Certificados (`winapp cert`)

Gera certificados de desenvolvimento e instala no store local da maquina para assinar pacotes.

### 6. Assinatura de Pacotes (`winapp sign`)

Assina pacotes MSIX e executaveis com certificados PFX, com suporte opcional a timestamp server.

### 7. Acesso a Ferramentas de Build do SDK (`winapp tool`)

Executa ferramentas de build do Windows SDK com paths corretamente configurados a partir de qualquer framework ou sistema de build.

## Exemplos de Uso

### Exemplo 1: Inicializar e Empacotar um App Windows

```bash
# Initialize workspace with defaults
winapp init

# Build your application (framework-specific)
# ...

# Create signed MSIX package
winapp pack ./build-output --generate-cert --output MyApp.msix
```

### Exemplo 2: Debug com Package Identity

```bash
# Add debug identity to executable for testing Windows APIs
winapp create-debug-identity ./bin/MyApp.exe

# Run your app - it now has package identity
./bin/MyApp.exe
```

### Exemplo 3: Configuracao de Pipeline CI/CD

```yaml
# GitHub Actions example
- name: Configurar winapp CLI
  uses: microsoft/setup-WinAppCli@v1

- name: Initialize and Package
  run: |
    winapp init --no-prompt
    winapp pack ./build-output --output MyApp.msix
```

### Exemplo 4: Integracao com App Electron

```bash
# Install via npm
npm install @microsoft/winappcli --save-dev

# Initialize and add debug identity for Electron
npx winapp init
npx winapp node add-electron-debug-identity

# Package for distribution
npx winapp pack ./out --output MyElectronApp.msix
```

## Diretrizes

1. **Execute `winapp init` primeiro** - sempre inicialize o projeto antes de usar outros comandos para garantir SDK, manifest e certificados configurados.
2. **Reexecute `create-debug-identity` apos mudancas no manifest** - a package identity precisa ser recriada quando AppxManifest.xml for modificado.
3. **Use `--no-prompt` em CI/CD** - evita prompts interativos em pipelines automatizados usando valores padrao.
4. **Use `winapp restore` em projetos compartilhados** - recria o estado exato do ambiente definido em `winapp.yaml` em outras maquinas.
5. **Gere assets a partir de uma unica imagem** - use `winapp manifest update-assets` com um logo para gerar todos os tamanhos exigidos.

## Padroes Comuns

### Padrao: Inicializar Novo Projeto

```bash
cd my-project
winapp init
# Creates: AppxManifest.xml, development certificate, SDK configuration, winapp.yaml
```

### Padrao: Empacotar com Certificado Existente

```bash
winapp pack ./build-output --cert ./mycert.pfx --cert-password secret --output MyApp.msix
```

### Padrao: Deploy Self-Contained

```bash
# Bundle Windows App SDK runtime with the package
winapp pack ./my-app --self-contained --generate-cert
```

### Padrao: Atualizar Versoes de Pacote

```bash
# Update to latest stable SDKs
winapp update

# Or update to preview SDKs
winapp update --setup-sdks preview
```

## Limitacoes

- Windows 10 ou superior (CLI apenas Windows)
- Debug com package identity exige reexecutar `create-debug-identity` apos qualquer mudanca no manifest
- Deploy self-contained aumenta o tamanho do pacote por incluir o runtime do Windows App SDK
- Certificados de desenvolvimento sao apenas para teste; producao exige certificados confiaveis
- Algumas APIs Windows exigem declaracoes de capabilities especificas no manifest
- winapp CLI esta em public preview e sujeito a mudancas

## Windows APIs Habilitadas por Package Identity

Package identity libera acesso a APIs poderosas do Windows:

| Categoria de API | Exemplos |
| --------------- | -------- |
| **Notifications** | Interactive native notifications, notification management |
| **Windows AI** | On-device LLM, text/image AI APIs (Phi Silica, Windows ML) |
| **Shell Integration** | Explorer, Taskbar, Share sheet integration |
| **Protocol Handlers** | Custom URI schemes (`yourapp://`) |
| **Device Access** | Camera, microphone, location (with consent) |
| **Background Tasks** | Run when app is closed |
| **File Associations** | Open file types with your app |

## Solucao de Problemas

| Problema | Solucao |
| -------- | ------- |
| Certificate not trusted | Execute `winapp cert install <cert-path>` para instalar no store local |
| Package identity not working | Execute `winapp create-debug-identity` apos qualquer mudanca no manifest |
| SDK not found | Execute `winapp restore` ou `winapp update` para garantir SDKs instalados |
| Signing fails | Verifique a senha do certificado e se ele nao expirou |

## Referencias

- [GitHub Repository](https://github.com/microsoft/WinAppCli)
- [Full CLI Documentation](https://github.com/microsoft/WinAppCli/blob/main/docs/usage.md)
- [Sample Applications](https://github.com/microsoft/WinAppCli/tree/main/samples)
- [Windows App SDK](https://learn.microsoft.com/windows/apps/windows-app-sdk/)
- [Visao Geral de Empacotamento MSIX](https://learn.microsoft.com/windows/msix/overview)
- [Visao Geral de Package Identity](https://learn.microsoft.com/windows/apps/desktop/modernize/package-identity-overview)
