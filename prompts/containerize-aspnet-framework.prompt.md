---
agent: 'agent'
tools: ['search/codebase', 'edit/editFiles', 'terminalCommand']
description: 'Containerize um projeto ASP.NET .NET Framework criando Dockerfile e arquivos .dockerfile customizados para o projeto.'
---

# Prompt de Containerizacao de ASP.NET .NET Framework

Containerize o projeto ASP.NET (.NET Framework) especificado nas configuracoes de containerizacao abaixo, focando **exclusivamente** nas mudancas necessarias para o app rodar em um container Docker Windows. A containerizacao deve considerar todas as configuracoes especificadas aqui.

**LEMBRETE:** Este e um app .NET Framework, nao .NET Core. O processo de containerizacao sera diferente do de um app .NET Core.

## Configuracoes de Containerizacao

Esta secao do prompt contem as configuracoes especificas e os ajustes necessarios para containerizar a aplicacao ASP.NET (.NET Framework). Antes de rodar este prompt, garanta que as configuracoes estejam preenchidas com as informacoes necessarias. Em muitos casos, apenas as primeiras configuracoes sao necessarias. Configuracoes posteriores podem ficar como default se nao se aplicarem ao projeto.

Quaisquer configuracoes nao especificadas serao definidas como default. Os valores padrao estao em `[colchetes]`.

### Informacoes Basicas do Projeto
1. Projeto para containerizar:
   - `[ProjectName (provide path to .csproj file)]`

2. Windows Server SKU a usar:
   - `[Windows Server Core (Default) or Windows Server Full]`

3. Versao do Windows Server a usar:
   - `[2022, 2019, or 2016 (Default 2022)]`

4. Imagem base customizada para o stage de build da imagem Docker ("None" para usar imagem base Microsoft padrao):
   - `[Specify base image to use for build stage (Default None)]`

5. Imagem base customizada para o stage de run da imagem Docker ("None" para usar imagem base Microsoft padrao):
   - `[Specify base image to use for run stage (Default None)]`

### Configuracao do Container
1. Ports que devem ser expostas na imagem do container:
   - Primary HTTP port: `[e.g., 80]`
   - Additional ports: `[List any additional ports, or "None"]`

2. Conta de usuario que o container deve usar:
   - `[User account, or default to "ContainerUser"]`

3. Configuracoes de IIS que devem ser definidas na imagem do container:
   - `[List any specific IIS settings, or "None"]`

### Configuracao de Build
1. Passos de build customizados que devem ser executados antes de buildar a imagem do container:
   - `[List any specific build steps, or "None"]`

2. Passos de build customizados que devem ser executados apos buildar a imagem do container:
   - `[List any specific build steps, or "None"]`

### Dependencias
1. Assemblies .NET que devem ser registrados no GAC na imagem do container:
   - `[Assembly name and version, or "None"]`

2. MSIs que devem ser copiados para a imagem do container e instalados:
   - `[MSI names and versions, or "None"]`

3. Componentes COM que devem ser registrados na imagem do container:
   - `[COM component names, or "None"]`

### Configuracao do Sistema
1. Chaves e valores de registro que devem ser adicionados a imagem do container:
   - `[Registry paths and values, or "None"]`

2. Variaveis de ambiente que devem ser definidas na imagem do container:
   - `[Variable names and values, or "Use defaults"]`

3. Roles e features do Windows Server que devem ser instaladas na imagem do container:
   - `[Role/feature names, or "None"]`

### Sistema de Arquivos
1. Arquivos/diretorios que precisam ser copiados para a imagem do container:
   - `[Paths relative to project root, or "None"]`
   - Target location in container: `[Container paths, or "Not applicable"]`

2. Arquivos/diretorios a excluir da containerizacao:
   - `[Paths to exclude, or "None"]`

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

- ✅ Modificacao de configuracao do app para garantir que config builders leiam app settings e connection strings de variaveis de ambiente
- ✅ Criacao e configuracao de Dockerfile para um app ASP.NET
- ✅ Especificacao de multiplos stages no Dockerfile para build/publish e copia do output para a imagem final
- ✅ Configuracao de compatibilidade da plataforma de containers Windows (Windows Server Core ou Full)
- ✅ Tratamento adequado de dependencias (GAC assemblies, MSIs, COM components)
- ❌ Sem setup de infraestrutura (assumido como separado)
- ❌ Sem mudancas de codigo alem das necessarias para containerizacao

## Processo de Execucao

1. Revise as configuracoes de containerizacao acima para entender os requisitos
2. Crie um arquivo `progress.md` para rastrear mudancas com check marks
3. Determine a versao do .NET Framework no .csproj verificando o elemento `TargetFrameworkVersion`
4. Selecione a imagem apropriada do Windows Server com base em:
   - A versao do .NET Framework detectada no projeto
   - O Windows Server SKU especificado nas configuracoes (Core ou Full)
   - A versao do Windows Server especificada nas configuracoes (2016, 2019 ou 2022)
   - Windows Server Core tags podem ser encontradas em: https://github.com/microsoft/dotnet-framework-docker/blob/main/README.aspnet.md#full-tag-listing
5. Garanta que os pacotes NuGet necessarios estao instalados. **NAO** instale esses pacotes se estiverem ausentes. Se estiverem ausentes, o usuario deve instalar manualmente. Se nao estiverem instalados, pause este prompt e peca ao usuario para instalar usando o Visual Studio NuGet Package Manager ou o Visual Studio package manager console. Os seguintes pacotes sao obrigatorios:
   - `Microsoft.Configuration.ConfigurationBuilders.Environment`
6. Modifique o `web.config` para adicionar a secao de configuration builders e settings para ler app settings e connection strings de variaveis de ambiente:
   - Adicione a secao ConfigBuilders em configSections
   - Adicione a secao configBuilders na raiz
   - Configure EnvironmentConfigBuilder para appSettings e connectionStrings
   - Example pattern:
     ```xml
     <configSections>
       <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
     </configSections>
     <configBuilders>
       <builders>
         <add name="Environment" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
       </builders>
     </configBuilders>
     <appSettings configBuilders="Environment">
       <!-- existing app settings -->
     </appSettings>
     <connectionStrings configBuilders="Environment">
       <!-- existing connection strings -->
     </connectionStrings>
     ```
7. Crie um arquivo `LogMonitorConfig.json` na pasta onde o Dockerfile sera criado copiando o arquivo de referencia `LogMonitorConfig.json` no final deste prompt. O conteudo do arquivo **NAO DEVE** ser modificado e deve corresponder exatamente ao conteudo de referencia, a menos que as instrucoes de containerizacao digam o contrario.
   - Em particular, garanta que o nivel de issues a serem logadas nao seja alterado, pois usar `Information` para EventLog causa ruido desnecessario.
8. Crie um Dockerfile na raiz do projeto para containerizar a aplicacao
   - O Dockerfile deve usar multiplos stages:
     - Build stage: Use uma imagem Windows Server Core para buildar a aplicacao
       - O build stage **DEVE** usar uma imagem base `mcr.microsoft.com/dotnet/framework/sdk` a menos que uma imagem custom seja especificada nas settings
       - Copie arquivos sln, csproj e packages.config primeiro
       - Copie NuGet.config se existir e configure feeds privados
       - Restaure pacotes NuGet
       - Em seguida, copie o restante do codigo-fonte e compile e publique a aplicacao em C:\publish via MSBuild
     - Final stage: Use a imagem Windows Server selecionada para rodar a aplicacao
       - O final stage **DEVE** usar uma imagem base `mcr.microsoft.com/dotnet/framework/aspnet` a menos que uma imagem custom seja especificada nas settings
       - Copie o arquivo `LogMonitorConfig.json` para um diretorio no container (ex.: C:\LogMonitor)
       - Baixe o LogMonitor.exe do repositorio da Microsoft para o mesmo diretorio
           - A URL correta do LogMonitor.exe e: https://github.com/microsoft/windows-container-tools/releases/download/v2.1.1/LogMonitor.exe
       - Defina o working directory como C:\inetpub\wwwroot
       - Copie o output publicado do build stage (em C:\publish) para a imagem final
       - Defina o entry point do container para rodar LogMonitor.exe com ServiceMonitor.exe e monitorar o IIS
           - `ENTRYPOINT [ "C:\\LogMonitor\\LogMonitor.exe", "C:\\ServiceMonitor.exe", "w3svc" ]`
   - Considere todos os requisitos nas configuracoes de containerizacao:
     - Windows Server SKU e versao
     - Ports expostas
     - User account do container
     - IIS settings
     - Registro de assemblies no GAC
     - Instalacao de MSI
     - Registro de COM components
     - Registry keys
     - Environment variables
     - Windows roles e features
     - Copia de arquivos/diretorios
   - Modele o Dockerfile com base no exemplo ao final deste prompt, mas assegure que seja customizado para os requisitos e settings especificas.
   - **IMPORTANTE:** Use uma imagem base Windows Server Core a menos que o usuario tenha **especificamente** solicitado Windows Server Full nas settings
9. Crie um arquivo `.dockerignore` na raiz do projeto para excluir arquivos desnecessarios da imagem. O `.dockerignore` **DEVE** incluir ao menos os seguintes itens, bem como padroes adicionais especificados nas configuracoes:
   - packages/
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
   - Quaisquer padroes adicionais especificados nas configuracoes
10. Configure health checks se especificado nas settings:
   - Adicione instrucao HEALTHCHECK ao Dockerfile se um endpoint for fornecido
11. Adicione o Dockerfile ao projeto adicionando o seguinte item no arquivo do projeto: `<None Include="Dockerfile" />`
12. Marque tarefas como concluidas: [ ] → [✓]
13. Continue ate que todas as tarefas estejam concluidas e o build do Docker tenha sucesso

## Verificacao de Build e Runtime

Confirme que o build do Docker foi bem-sucedido assim que o Dockerfile estiver completo. Use o comando abaixo para buildar a imagem:

```bash
docker build -t aspnet-app:latest .
```

Se o build falhar, revise as mensagens de erro e faca ajustes necessarios no Dockerfile ou na configuracao do projeto. Reporte sucesso/falha.

## Acompanhamento de Progresso

Mantenha um arquivo `progress.md` com a seguinte estrutura:
```markdown
# Progresso de Containerizacao

## Deteccao de Ambiente
- [ ] Deteccao da versao do .NET Framework (version: ___)
- [ ] Selecao do Windows Server SKU (SKU: ___)
- [ ] Selecao da versao do Windows Server (Version: ___)

## Mudancas de Configuracao
- [ ] Modificacoes no Web.config para configuration builders
- [ ] Configuracao de origem de pacotes NuGet (se aplicavel)
- [ ] Copia de LogMonitorConfig.json e ajustes se exigidos pelas settings

## Containerizacao
- [ ] Criacao do Dockerfile
- [ ] Criacao do arquivo .dockerignore
- [ ] Build stage criado com imagem SDK
- [ ] sln, csproj, packages.config e (se aplicavel) NuGet.config copiados para restore de pacotes
- [ ] Runtime stage criado com imagem de runtime
- [ ] Configuracao de usuario non-root
- [ ] Tratamento de dependencias (GAC, MSI, COM, registry, arquivos adicionais, etc.)
- [ ] Configuracao de health check (se aplicavel)
- [ ] Implementacao de requisitos especiais

## Verificacao
- [ ] Revisar settings de containerizacao e garantir que todos os requisitos foram atendidos
- [ ] Sucesso no Docker build
```

Nao pare para confirmacao entre as etapas. Continue metodicamente ate que a aplicacao esteja containerizada e o Docker build tenha sucesso.

**VOCE NAO TERMINOU ATE QUE TODOS OS CHECKBOXES ESTEJAM MARCADOS!** Isso inclui buildar a imagem Docker com sucesso e resolver quaisquer issues que surjam durante o build.

## Materiais de Referencia

### Dockerfile de Exemplo

Um exemplo de Dockerfile para um app ASP.NET (.NET Framework) usando uma imagem base Windows Server Core.

```dockerfile
# escape=`
# The escape directive changes the escape character from \ to `
# This is especially useful in Windows Dockerfiles where \ is the path separator

# ============================================================
# Stage 1: Build and publish the application
# ============================================================

# Base Image - Select the appropriate .NET Framework version and Windows Server Core version
# Possible tags include:
# - 4.8.1-windowsservercore-ltsc2025 (Windows Server 2025)
# - 4.8-windowsservercore-ltsc2022 (Windows Server 2022)
# - 4.8-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.8-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.2-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.7.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.1-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.6.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 3.5-windowsservercore-ltsc2025 (Windows Server 2025)
# - 3.5-windowsservercore-ltsc2022 (Windows Server 2022)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2019)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2016)
# Uses the .NET Framework SDK image for building the application
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2022 AS build
ARG BUILD_CONFIGURATION=Release

# Set the default shell to PowerShell
SHELL ["powershell", "-command"]

WORKDIR /app

# Copy the solution and project files
COPY YourSolution.sln .
COPY YourProject/*.csproj ./YourProject/
COPY YourOtherProject/*.csproj ./YourOtherProject/

# Copy packages.config files
COPY YourProject/packages.config ./YourProject/
COPY YourOtherProject/packages.config ./YourOtherProject/

# Restore NuGet packages
RUN nuget restore YourSolution.sln

# Copy source code
COPY . .

# Perform custom pre-build steps here, if needed

# Build and publish the application to C:\publish
RUN msbuild /p:Configuration=$BUILD_CONFIGURATION `
            /p:WebPublishMethod=FileSystem `
            /p:PublishUrl=C:\publish `
            /p:DeployDefaultTarget=WebPublish

# Perform custom post-build steps here, if needed

# ============================================================
# Stage 2: Final runtime image
# ============================================================

# Base Image - Select the appropriate .NET Framework version and Windows Server Core version
# Possible tags include:
# - 4.8.1-windowsservercore-ltsc2025 (Windows Server 2025)
# - 4.8-windowsservercore-ltsc2022 (Windows Server 2022)
# - 4.8-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.8-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.2-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.7.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.1-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.6.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 3.5-windowsservercore-ltsc2025 (Windows Server 2025)
# - 3.5-windowsservercore-ltsc2022 (Windows Server 2022)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2019)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2016)
# Uses the .NET Framework ASP.NET image for running the application
FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2022

# Set the default shell to PowerShell
SHELL ["powershell", "-command"]

WORKDIR /inetpub/wwwroot

# Copy from build stage
COPY --from=build /publish .

# Add any additional environment variables needed for your application (uncomment and modify as needed)
# ENV KEY=VALUE

# Install MSI packages (uncomment and modify as needed)
# COPY ./msi-installers C:/Installers
# RUN Start-Process -Wait -FilePath 'msiexec.exe' -ArgumentList '/i', 'C:\Installers\your-package.msi', '/quiet', '/norestart'

# Install custom Windows Server roles and features (uncomment and modify as needed)
# RUN dism /Online /Enable-Feature /FeatureName:YOUR-FEATURE-NAME

# Add additional Windows features (uncomment and modify as needed)
# RUN Add-WindowsFeature Some-Windows-Feature; `
#    Add-WindowsFeature Another-Windows-Feature

# Install MSI packages if needed (uncomment and modify as needed)
# COPY ./msi-installers C:/Installers
# RUN Start-Process -Wait -FilePath 'msiexec.exe' -ArgumentList '/i', 'C:\Installers\your-package.msi', '/quiet', '/norestart'

# Register assemblies in GAC if needed (uncomment and modify as needed)
# COPY ./assemblies C:/Assemblies
# RUN C:\Windows\Microsoft.NET\Framework64\v4.0.30319\gacutil -i C:/Assemblies/YourAssembly.dll

# Register COM components if needed (uncomment and modify as needed)
# COPY ./com-components C:/Components
# RUN regsvr32 /s C:/Components/YourComponent.dll

# Add registry keys if needed (uncomment and modify as needed)
# RUN New-Item -Path 'HKLM:\Software\YourApp' -Force; `
#     Set-ItemProperty -Path 'HKLM:\Software\YourApp' -Name 'Setting' -Value 'Value'

# Configure IIS settings if needed (uncomment and modify as needed)
# RUN Import-Module WebAdministration; `
#     Set-ItemProperty 'IIS:\AppPools\DefaultAppPool' -Name somePropertyName -Value 'SomePropertyValue'; `
#     Set-ItemProperty 'IIS:\Sites\Default Web Site' -Name anotherPropertyName -Value 'AnotherPropertyValue'

# Expose necessary ports - By default, IIS uses port 80
EXPOSE 80
# EXPOSE 443  # Uncomment if using HTTPS

# Copy LogMonitor from the microsoft/windows-container-tools repository
WORKDIR /LogMonitor
RUN curl -fSLo LogMonitor.exe https://github.com/microsoft/windows-container-tools/releases/download/v2.1.1/LogMonitor.exe

# Copy LogMonitorConfig.json from local files
COPY LogMonitorConfig.json .

# Set non-administrator user
USER ContainerUser

# Override the container's default entry point to take advantage of the LogMonitor
ENTRYPOINT [ "C:\\LogMonitor\\LogMonitor.exe", "C:\\ServiceMonitor.exe", "w3svc" ]
```

## Adaptando este Exemplo

**Nota:** Customize este template com base nos requisitos especificos nas configuracoes de containerizacao.

Ao adaptar este exemplo de Dockerfile:

1. Substitua `YourSolution.sln`, `YourProject.csproj`, etc. pelos seus nomes reais de arquivos
2. Ajuste as versoes de Windows Server e .NET Framework conforme necessario
3. Modifique as etapas de instalacao de dependencias conforme necessario e remova o que nao for aplicavel
4. Adicione ou remova stages conforme o seu workflow
