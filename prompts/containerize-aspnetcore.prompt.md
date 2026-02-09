---
agent: 'agent'
tools: ['search/codebase', 'edit/editFiles', 'terminalCommand']
description: 'Containerize um projeto ASP.NET Core criando Dockerfile e arquivos .dockerfile customizados para o projeto.'
---

# Prompt de Containerizacao Docker para ASP.NET Core

## Solicitacao de Containerizacao

Containerize o projeto ASP.NET Core (.NET) especificado nas settings abaixo, focando **exclusivamente** nas mudancas necessarias para o app rodar em um container Docker Linux. A containerizacao deve considerar todas as settings especificadas aqui.

Siga as best practices para containerizar aplicacoes .NET Core, garantindo que o container seja otimizado para performance, seguranca e maintainability.

## Configuracoes de Containerizacao

Esta secao do prompt contem as settings e configuracoes necessarias para containerizar a aplicacao ASP.NET Core. Antes de rodar este prompt, garanta que as settings estejam preenchidas com as informacoes necessarias. Em muitos casos, apenas as primeiras settings sao necessarias. As posteriores podem ficar como default se nao se aplicarem ao projeto.

Quaisquer settings nao especificadas serao definidas como default. Os valores padrao estao em `[colchetes]`.

### Informacoes Basicas do Projeto
1. Projeto para containerizar:
   - `[ProjectName (provide path to .csproj file)]`

2. Versao do .NET a usar:
   - `[8.0 or 9.0 (Default 8.0)]`

3. Distribuicao Linux a usar:
   - `[debian, alpine, ubuntu, chiseled, or Azure Linux (mariner) (Default debian)]`

4. Imagem base customizada para o stage de build da imagem Docker ("None" para usar imagem base Microsoft padrao):
   - `[Specify base image to use for build stage (Default None)]`

5. Imagem base customizada para o stage de run da imagem Docker ("None" para usar imagem base Microsoft padrao):
   - `[Specify base image to use for run stage (Default None)]`

### Configuracao do Container
1. Ports que devem ser expostas na imagem do container:
   - Primary HTTP port: `[e.g., 8080]`
   - Additional ports: `[List any additional ports, or "None"]`

2. Conta de usuario que o container deve usar:
   - `[User account, or default to "$APP_UID"]`

3. Configuracao de URL da aplicacao:
   - `[Specify ASPNETCORE_URLS, or default to "http://+:8080"]`

### Configuracao de Build
1. Passos de build customizados que devem ser executados antes de buildar a imagem do container:
   - `[List any specific build steps, or "None"]`

2. Passos de build customizados que devem ser executados apos buildar a imagem do container:
   - `[List any specific build steps, or "None"]`

3. Fontes de pacotes NuGet que devem ser configuradas:
   - `[List any private NuGet feeds with authentication details, or "None"]`

### Dependencias
1. Pacotes de sistema que devem ser instalados na imagem do container:
   - `[Package names for the chosen Linux distribution, or "None"]`

2. Bibliotecas nativas que devem ser copiadas para a imagem do container:
   - `[Library names and paths, or "None"]`

3. Ferramentas .NET adicionais que devem ser instaladas:
   - `[Tool names and versions, or "None"]`

### Configuracao do Sistema
1. Variaveis de ambiente que devem ser definidas na imagem do container:
   - `[Variable names and values, or "Use defaults"]`

### Sistema de Arquivos
1. Arquivos/diretorios que precisam ser copiados para a imagem do container:
   - `[Paths relative to project root, or "None"]`
   - Target location in container: `[Container paths, or "Not applicable"]`

2. Arquivos/diretorios a excluir da containerizacao:
   - `[Paths to exclude, or "None"]`

3. Pontos de montagem de volume que devem ser configurados:
   - `[Volume paths for persistent data, or "None"]`

### Configuracao de .dockerignore
1. Padroes a incluir no arquivo `.dockerignore` (.dockerignore ja tera defaults comuns; estes sao padroes adicionais):
   - Additional patterns: `[List any additional patterns, or "None"]`

### Configuracao de Health Check
1. Endpoint de health check:
   - `[Health check URL path, or "None"]`

2. Intervalo e timeout do health check:
   - `[Interval and timeout values, or "Use defaults"]`

### Instrucoes Adicionais
1. Outras instrucoes que devem ser seguidas para containerizar o projeto:
   - `[Specific requirements, or "None"]`

2. Issues conhecidas para enderecar:
   - `[Describe any known issues, or "None"]`

## Escopo

- ✅ Modificacao de configuracao do app para garantir que application settings e connection strings possam ser lidos de variaveis de ambiente
- ✅ Criacao e configuracao de Dockerfile para um app ASP.NET Core
- ✅ Especificacao de multiplos stages no Dockerfile para build/publish e copia do output para a imagem final
- ✅ Configuracao de compatibilidade da plataforma de containers Linux (Alpine, Ubuntu, Chiseled ou Azure Linux (Mariner))
- ✅ Tratamento adequado de dependencias (system packages, native libraries, ferramentas adicionais)
- ❌ Sem setup de infraestrutura (assumido como separado)
- ❌ Sem mudancas de codigo alem das necessarias para containerizacao

## Processo de Execucao

1. Revise as settings de containerizacao acima para entender os requisitos
2. Crie um arquivo `progress.md` para rastrear mudancas com check marks
3. Determine a versao do .NET no .csproj verificando o elemento `TargetFramework`
4. Selecione a imagem Linux apropriada com base em:
   - A versao do .NET detectada no projeto
   - A distribuicao Linux especificada nas settings (Alpine, Ubuntu, Chiseled ou Azure Linux (Mariner))
   - Se o usuario nao solicitar imagens base especificas nas settings, as imagens base **DEVEM** ser imagens validas de mcr.microsoft.com/dotnet com tag conforme o exemplo abaixo ou na documentacao
   - Imagens oficiais Microsoft .NET para build e runtime:
      - SDK image tags (for build stage): https://github.com/dotnet/dotnet-docker/blob/main/README.sdk.md
      - ASP.NET Core runtime image tags: https://github.com/dotnet/dotnet-docker/blob/main/README.aspnet.md
      - .NET runtime image tags: https://github.com/dotnet/dotnet-docker/blob/main/README.runtime.md
5. Crie um Dockerfile na raiz do projeto para containerizar a aplicacao
   - O Dockerfile deve usar multiplos stages:
     - Build stage: Use uma imagem .NET SDK para buildar a aplicacao
       - Copie arquivos csproj primeiro
       - Copie NuGet.config se existir e configure feeds privados
       - Restaure pacotes NuGet
       - Em seguida, copie o restante do codigo-fonte e compile e publique a aplicacao em /app/publish
     - Final stage: Use a imagem .NET runtime selecionada para rodar a aplicacao
       - Defina o working directory como /app
       - Defina o user conforme direcionado (por default, um user non-root (ex.: `$APP_UID`))
         - A menos que as settings indiquem o contrario, nao e necessario criar um novo user. Use a variavel `$APP_UID` para especificar a conta.
       - Copie o output publicado do build stage para a imagem final
   - Considere todos os requisitos nas settings de containerizacao:
     - .NET version e Linux distribution
     - Ports expostas
     - User account do container
     - Configuracao ASPNETCORE_URLS
     - Instalacao de system packages
     - Dependencias de native library
     - Ferramentas .NET adicionais
     - Environment variables
     - Copia de arquivos/diretorios
     - Volume mount points
     - Health check configuration
6. Crie um arquivo `.dockerignore` na raiz do projeto para excluir arquivos desnecessarios da imagem. O `.dockerignore` **DEVE** incluir ao menos os seguintes itens, bem como padroes adicionais especificados nas settings:
   - bin/
   - obj/
   - .dockerignore
   - Dockerfile
   - .git/
   - .github/
   - .vs/
   - .vscode/
   - **/node_modules/
   - *.user
   - *.suo
   - **/.DS_Store
   - **/Thumbs.db
   - Quaisquer padroes adicionais especificados nas settings
7. Configure health checks se especificado nas settings:
   - Adicione instrucao HEALTHCHECK ao Dockerfile se um endpoint de health check for fornecido
   - Use curl ou wget para checar o endpoint
8. Marque tarefas como concluidas: [ ] → [✓]
9. Continue ate que todas as tarefas estejam concluidas e o Docker build tenha sucesso

## Verificacao de Build e Runtime

Confirme que o Docker build foi bem-sucedido assim que o Dockerfile estiver completo. Use o comando abaixo para buildar a imagem:

```bash
docker build -t aspnetcore-app:latest .
```

Se o build falhar, revise as mensagens de erro e faca ajustes necessarios no Dockerfile ou na configuracao do projeto. Reporte sucesso/falha.

## Acompanhamento de Progresso

Mantenha um arquivo `progress.md` com a seguinte estrutura:
```markdown
# Progresso de Containerizacao

## Deteccao de Ambiente
- [ ] Deteccao da versao do .NET (version: ___)
- [ ] Selecao da distribuicao Linux (distribution: ___)

## Mudancas de Configuracao
- [ ] Verificacao de configuracao da aplicacao para suporte a variaveis de ambiente
- [ ] Configuracao de origem de pacotes NuGet (se aplicavel)

## Containerizacao
- [ ] Criacao do Dockerfile
- [ ] Criacao do arquivo .dockerignore
- [ ] Build stage criado com imagem SDK
- [ ] Arquivo(s) csproj copiados para restore de pacotes
- [ ] NuGet.config copiado se aplicavel
- [ ] Runtime stage criado com imagem de runtime
- [ ] Configuracao de usuario non-root
- [ ] Tratamento de dependencias (system packages, native libraries, tools, etc.)
- [ ] Configuracao de health check (se aplicavel)
- [ ] Implementacao de requisitos especiais

## Verificacao
- [ ] Revisar settings de containerizacao e garantir que todos os requisitos foram atendidos
- [ ] Sucesso no Docker build
```

Nao pare para confirmacao entre as etapas. Continue metodicamente ate que a aplicacao esteja containerizada e o Docker build tenha sucesso.

**VOCE NAO TERMINOU ATE QUE TODOS OS CHECKBOXES ESTEJAM MARCADOS!** Isso inclui buildar a imagem Docker com sucesso e resolver quaisquer issues que surjam durante o build.

## Exemplo de Dockerfile

Um exemplo de Dockerfile para um app ASP.NET Core (.NET) usando uma imagem base Linux.

```dockerfile
# ============================================================
# Stage 1: Build and publish the application
# ============================================================

# Base Image - Select the appropriate .NET SDK version and Linux distribution
# Possible tags include:
# - 8.0-bookworm-slim (Debian 12)
# - 8.0-noble (Ubuntu 24.04)
# - 8.0-alpine (Alpine Linux)
# - 9.0-bookworm-slim (Debian 12)
# - 9.0-noble (Ubuntu 24.04)
# - 9.0-alpine (Alpine Linux)
# Uses the .NET SDK image for building the application
FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build
ARG BUILD_CONFIGURATION=Release

WORKDIR /src

# Copy project files first for better caching
COPY ["YourProject/YourProject.csproj", "YourProject/"]
COPY ["YourOtherProject/YourOtherProject.csproj", "YourOtherProject/"]

# Copy NuGet configuration if it exists
COPY ["NuGet.config", "."]

# Restore NuGet packages
RUN dotnet restore "YourProject/YourProject.csproj"

# Copy source code
COPY . .

# Perform custom pre-build steps here, if needed
# RUN echo "Running pre-build steps..."

# Build and publish the application
WORKDIR "/src/YourProject"
RUN dotnet build "YourProject.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish the application
RUN dotnet publish "YourProject.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Perform custom post-build steps here, if needed
# RUN echo "Running post-build steps..."

# ============================================================
# Stage 2: Final runtime image
# ============================================================

# Base Image - Select the appropriate .NET runtime version and Linux distribution
# Possible tags include:
# - 8.0-bookworm-slim (Debian 12)
# - 8.0-noble (Ubuntu 24.04)
# - 8.0-alpine (Alpine Linux)
# - 8.0-noble-chiseled (Ubuntu 24.04 Chiseled)
# - 8.0-azurelinux3.0 (Azure Linux)
# - 9.0-bookworm-slim (Debian 12)
# - 9.0-noble (Ubuntu 24.04)
# - 9.0-alpine (Alpine Linux)
# - 9.0-noble-chiseled (Ubuntu 24.04 Chiseled)
# - 9.0-azurelinux3.0 (Azure Linux)
# Uses the .NET runtime image for running the application
FROM mcr.microsoft.com/dotnet/aspnet:8.0-bookworm-slim AS final

# Install system packages if needed (uncomment and modify as needed)
# RUN apt-get update && apt-get install -y \
#     curl \
#     wget \
#     ca-certificates \
#     libgdiplus \
#     && rm -rf /var/lib/apt/lists/*

# Install additional .NET tools if needed (uncomment and modify as needed)
# RUN dotnet tool install --global dotnet-ef --version 8.0.0
# ENV PATH="$PATH:/root/.dotnet/tools"

WORKDIR /app

# Copy published application from build stage
COPY --from=build /app/publish .

# Copy additional files if needed (uncomment and modify as needed)
# COPY ./config/appsettings.Production.json .
# COPY ./certificates/ ./certificates/

# Set environment variables
ENV ASPNETCORE_ENVIRONMENT=Production
ENV ASPNETCORE_URLS=http://+:8080

# Add custom environment variables if needed (uncomment and modify as needed)
# ENV CONNECTIONSTRINGS__DEFAULTCONNECTION="your-connection-string"
# ENV FEATURE_FLAG_ENABLED=true

# Configure SSL/TLS certificates if needed (uncomment and modify as needed)
# ENV ASPNETCORE_Kestrel__Certificates__Default__Path=/app/certificates/app.pfx
# ENV ASPNETCORE_Kestrel__Certificates__Default__Password=your_password

# Expose the port the application listens on
EXPOSE 8080
# EXPOSE 8081  # Uncomment if using HTTPS

# Install curl for health checks if not already present
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# Configure health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:8080/health || exit 1

# Create volumes for persistent data if needed (uncomment and modify as needed)
# VOLUME ["/app/data", "/app/logs"]

# Switch to non-root user for security
USER $APP_UID

# Set the entry point for the application
ENTRYPOINT ["dotnet", "YourProject.dll"]
```

## Adaptando este Exemplo

**Nota:** Customize este template com base nos requisitos especificos nas configuracoes de containerizacao.

Ao adaptar este exemplo de Dockerfile:

1. Substitua `YourProject.csproj`, `YourProject.dll`, etc. pelos nomes reais do projeto
2. Ajuste a versao do .NET e a distribuicao Linux conforme necessario
3. Modifique as etapas de instalacao de dependencias conforme necessario e remova o que nao for aplicavel
4. Configure environment variables especificas do seu app
5. Adicione ou remova stages conforme o seu workflow
6. Atualize o health check endpoint para corresponder a rota de health check do app

## Variacoes por Distribuicao Linux

### Alpine Linux
Para imagens menores, voce pode usar Alpine Linux:

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0-alpine AS build
# ... build steps ...

FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine AS final
# Install packages using apk
RUN apk update && apk add --no-cache curl ca-certificates
```

### Ubuntu Chiseled
Para menor superficie de ataque, considere imagens chiseled:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0-jammy-chiseled AS final
# Note: Chiseled images have minimal packages, so you may need to use a different base for additional dependencies
```

### Azure Linux (Mariner)
Para containers otimizados para Azure:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0-azurelinux3.0 AS final
# Install packages using tdnf
RUN tdnf update -y && tdnf install -y curl ca-certificates && tdnf clean all
```

## Notas sobre Nomeacao de Stages

- A sintaxe `AS stage-name` da um nome a cada stage
- Use `--from=stage-name` para copiar arquivos de um stage anterior
- Voce pode ter multiplos stages intermediarios que nao sao usados na imagem final
- O stage `final` e o que se torna a imagem final do container

## Boas Praticas de Seguranca

- Sempre rode como non-root user em producao
- Use tags de imagem especificas em vez de `latest`
- Minimize o numero de pacotes instalados
- Mantenha imagens base atualizadas
- Use multi-stage builds para excluir dependencias de build da imagem final
