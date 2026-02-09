# Exemplos de Receita Executaveis

Esta pasta contem exemplos C# independentes e executaveis para cada receita do cookbook. Sao [file-based apps](https://learn.microsoft.com/dotnet/core/sdk/file-based-apps) que podem ser executadas diretamente com `dotnet run`.

## Pre-requisitos

- .NET 10.0 ou superior
- Pacote GitHub Copilot SDK (referenciado automaticamente)

## Executando Exemplos

Cada arquivo `.cs` e um programa completo e executavel. Basta usar:

```bash
dotnet run <filename>.cs
```

### Receitas Disponiveis

| Receita               | Comando                              | Descricao                                  |
| --------------------- | ------------------------------------ | ------------------------------------------ |
| Error Handling        | `dotnet run error-handling.cs`       | Demonstra padroes de tratamento de erros   |
| Multiple Sessions     | `dotnet run multiple-sessions.cs`    | Gerencia multiplas conversas independentes |
| Managing Local Files  | `dotnet run managing-local-files.cs` | Organiza arquivos com agrupamento por IA   |
| PR Visualization      | `dotnet run pr-visualization.cs`     | Gera graficos de idade de PR               |
| Persisting Sessions   | `dotnet run persisting-sessions.cs`  | Salva e retoma sessoes entre reinicios     |

### Exemplos com Argumentos

**PR Visualization com repo especifico:**

```bash
dotnet run pr-visualization.cs -- --repo github/copilot-sdk
```

**Managing Local Files (edite o arquivo para mudar a pasta alvo):**

```bash
# Edit the targetFolder variable in managing-local-files.cs first
dotnet run managing-local-files.cs
```

## File-Based Apps

Estes exemplos usam o recurso de file-based apps do .NET, que permite programas C# em arquivo unico para:

- Executar sem arquivo de projeto
- Referenciar automaticamente pacotes comuns
- Suportar top-level statements

## Recursos de Aprendizado

- [.NET File-Based Apps Documentation](https://learn.microsoft.com/en-us/dotnet/core/sdk/file-based-apps)
- [GitHub Copilot SDK Documentation](https://github.com/github/copilot-sdk/blob/main/dotnet/README.md)
- [Cookbook Pai](../README.md)
