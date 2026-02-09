# Exemplos de Receita Executaveis

Esta pasta contem exemplos Go independentes e executaveis para cada receita do cookbook. Cada arquivo e um programa completo que pode ser executado diretamente com `go run`.

## Pre-requisitos

- Go 1.21 ou superior
- GitHub Copilot SDK para Go

```bash
go get github.com/github/copilot-sdk/go
```

## Executando Exemplos

Cada arquivo `.go` e um programa completo e executavel. Basta usar:

```bash
go run <filename>.go
```

### Receitas Disponiveis

| Receita               | Comando                          | Descricao                                  |
| --------------------- | -------------------------------- | ------------------------------------------ |
| Error Handling        | `go run error-handling.go`       | Demonstra padroes de tratamento de erros   |
| Multiple Sessions     | `go run multiple-sessions.go`    | Gerencia multiplas conversas independentes |
| Managing Local Files  | `go run managing-local-files.go` | Organiza arquivos com agrupamento por IA   |
| PR Visualization      | `go run pr-visualization.go`     | Gera graficos de idade de PR               |
| Persisting Sessions   | `go run persisting-sessions.go`  | Salva e retoma sessoes entre reinicios     |

### Exemplos com Argumentos

**PR Visualization com repo especifico:**

```bash
go run pr-visualization.go -repo github/copilot-sdk
```

**Managing Local Files (edite o arquivo para mudar a pasta alvo):**

```bash
# Edit the targetFolder variable in managing-local-files.go first
go run managing-local-files.go
```

## Melhores Praticas em Go

Estes exemplos seguem convencoes de Go:

- Tratamento de erros adequado com verificacoes explicitas
- Uso de `defer` para limpeza
- Nomenclatura idiomatica (camelCase para variaveis locais)
- Uso da biblioteca padrao quando apropriado
- Separacao clara de responsabilidades

## Recursos de Aprendizado

- [Go Documentation](https://go.dev/doc/)
- [GitHub Copilot SDK for Go](https://github.com/github/copilot-sdk/blob/main/go/README.md)
- [Cookbook Pai](../README.md)
