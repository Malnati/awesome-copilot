---
name: nuget-manager
description: 'Gerencie pacotes NuGet em projetos/solucoes .NET. Use esta skill ao adicionar, remover ou atualizar versoes de pacotes NuGet. Ela reforca o uso do `dotnet` CLI para gerenciamento e define procedimentos estritos para edicoes diretas de arquivo apenas ao atualizar versoes.'
---

# Gerenciador do NuGet

## Visao Geral

Esta skill garante gerenciamento consistente e seguro de pacotes NuGet em projetos .NET. Ela prioriza o uso do `dotnet` CLI para manter a integridade do projeto e aplica um fluxo de trabalho (workflow) estrito de verificacao e restore para atualizacoes de versao.

## Pre-requisitos

- .NET SDK instalado (tipicamente .NET 8.0 SDK ou posterior, ou versao compativel com a solucao alvo).
- `dotnet` CLI disponivel no `PATH`.
- `jq` (processador JSON) OU PowerShell (para verificacao de versao usando `dotnet package search`).

## Regras Principais

1.  **NUNCA** edite diretamente `.csproj`, `.props` ou `Directory.Packages.props` para **adicionar** ou **remover** pacotes. Sempre use `dotnet add package` e `dotnet remove package`.
2.  **EDICAO DIRETA** e permitida APENAS para **alterar versoes** de pacotes existentes.
3.  **ATUALIZACOES DE VERSAO** devem seguir o fluxo de trabalho (workflow) obrigatorio:
    - Verificar se a versao alvo existe no NuGet.
    - Determinar se as versoes sao gerenciadas por projeto (`.csproj`) ou centralmente (`Directory.Packages.props`).
    - Atualizar a string de versao no arquivo apropriado.
    - Rodar `dotnet restore` imediatamente para verificar compatibilidade.

## Workflows

### Adicionar um Pacote
Use `dotnet add [<PROJECT>] package <PACKAGE_NAME> [--version <VERSION>]`.
Exemplo: `dotnet add src/MyProject/MyProject.csproj package Newtonsoft.Json`

### Remover um Pacote
Use `dotnet remove [<PROJECT>] package <PACKAGE_NAME>`.
Exemplo: `dotnet remove src/MyProject/MyProject.csproj package Newtonsoft.Json`

### Atualizar Versoes de Pacote
Ao atualizar uma versao, siga estes passos:

1.  **Verificar Existencia da Versao**:
    Cheque se a versao existe usando o comando `dotnet package search` com match exato e formato JSON.
    Usando `jq`:
    `dotnet package search <PACKAGE_NAME> --exact-match --format json | jq -e '.searchResult[].packages[] | select(.version == "<VERSION>")'`
    Usando PowerShell:
    `(dotnet package search <PACKAGE_NAME> --exact-match --format json | ConvertFrom-Json).searchResult.packages | Where-Object { $_.version -eq "<VERSION>" }`

2.  **Determinar Gerenciamento de Versao**:
    - Procure `Directory.Packages.props` na raiz da solucao. Se existir, versoes devem ser gerenciadas la via `<PackageVersion Include="Package.Name" Version="1.2.3" />`.
    - Se nao existir, verifique arquivos `.csproj` individuais para `<PackageReference Include="Package.Name" Version="1.2.3" />`.

3.  **Aplicar Mudancas**:
    Modifique o arquivo identificado com a nova string de versao.

4.  **Verificar Estabilidade**:
    Execute `dotnet restore` no projeto ou solucao. Se ocorrerem erros, reverta a mudanca e investigue.

## Exemplos

### Usuario: "Add Serilog to the WebApi project"
**Acao**: Execute `dotnet add src/WebApi/WebApi.csproj package Serilog`.

### Usuario: "Update Newtonsoft.Json to 13.0.3 in the whole solution"
**Acao**:
1. Verifique se 13.0.3 existe: `dotnet package search Newtonsoft.Json --exact-match --format json` (e parseie a saida para confirmar que "13.0.3" esta presente).
2. Encontre onde esta definido (ex.: `Directory.Packages.props`).
3. Edite o arquivo para atualizar a versao.
4. Rode `dotnet restore`.
