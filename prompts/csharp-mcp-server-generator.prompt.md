---
agent: 'agent'
description: 'Gere um projeto completo de servidor MCP em C# com tools, prompts e configuracao adequada'
---

# Gerar servidor MCP em C#

Crie um servidor completo de Model Context Protocol (MCP) em C# com as seguintes especificacoes:

## Requisitos

1. **Estrutura do Projeto**: Crie uma nova aplicacao console C# com estrutura de diretorios adequada
2. **Pacotes NuGet**: Inclua ModelContextProtocol (prerelease) e Microsoft.Extensions.Hosting
3. **Configuracao de Logging**: Configure todos os logs para stderr para nao interferir com transporte via stdio
4. **Setup do Servidor**: Use o padrão Host builder com configuracao DI adequada
5. **Tools**: Crie ao menos uma tool util com atributos e descricoes apropriadas
6. **Tratamento de Erros**: Inclua validacao e tratamento de erros adequados

## Detalhes de Implementacao

### Setup Basico do Projeto
- Use .NET 8.0 ou superior
- Crie uma aplicacao console
- Adicione pacotes NuGet necessarios com flag --prerelease
- Configure logging para stderr

### Configuracao do Servidor
- Use `Host.CreateApplicationBuilder` para DI e gerenciamento do lifecycle
- Configure `AddMcpServer()` com transporte stdio
- Use `WithToolsFromAssembly()` para descoberta automatica de tools
- Garanta que o servidor rode com `RunAsync()`

### Implementacao de Tools
- Use atributo `[McpServerToolType]` nas classes de tools
- Use atributo `[McpServerTool]` nos metodos de tools
- Adicione atributos `[Description]` em tools e parametros
- Suporte operacoes async quando apropriado
- Inclua validacao adequada de parametros

### Qualidade de Codigo
- Siga convencoes de nomenclatura C#
- Inclua comentarios XML de documentacao
- Use nullable reference types
- Implemente tratamento de erro apropriado com McpProtocolException
- Use logging estruturado para debug

## Tipos de Tools a Considerar
- Operacoes de arquivo (read, write, search)
- Processamento de dados (transform, validate, analyze)
- Integracoes com APIs externas (HTTP requests)
- Operacoes de sistema (executar comandos, checar status)
- Operacoes de banco de dados (query, update)

## Orientacao de Testes
- Explique como rodar o servidor
- Forneca comandos de exemplo para testar com clientes MCP
- Inclua dicas de troubleshooting

Gere um servidor MCP pronto para producao com documentacao e tratamento de erros abrangentes.
