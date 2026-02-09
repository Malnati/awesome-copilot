---
agent: 'agent'
description: 'Gere um projeto completo de servidor MCP em TypeScript com tools, resources e configuracao adequada'
---

# Gerar Servidor MCP em TypeScript

Crie um servidor Model Context Protocol (MCP) completo em TypeScript com as seguintes especificacoes:

## Requisitos

1. **Estrutura do Projeto**: Crie um novo projeto TypeScript/Node.js com estrutura de diretorios adequada
2. **Pacotes NPM**: Inclua @modelcontextprotocol/sdk, zod@3 e suporte a express (HTTP) ou stdio
3. **Configuracao TypeScript**: tsconfig.json adequado com suporte a ES modules
4. **Tipo de Servidor**: Escolha entre HTTP (com Streamable HTTP transport) ou stdio
5. **Tools**: Crie pelo menos uma tool util com validacao de schema adequada
6. **Error Handling**: Inclua tratamento de erros e validacao abrangentes

## Detalhes de Implementacao

### Setup do Projeto
- Inicialize com `npm init` e crie package.json
- Instale dependencias: `@modelcontextprotocol/sdk`, `zod@3` e pacotes do transport escolhido
- Configure TypeScript com ES modules: `"type": "module"` em package.json
- Adicione dev dependencies: `tsx` ou `ts-node` para desenvolvimento
- Crie arquivo .gitignore apropriado

### Configuracao do Servidor
- Use a classe `McpServer` para implementacao de alto nivel
- Defina nome e versao do servidor
- Escolha o transport apropriado (StreamableHTTPServerTransport ou StdioServerTransport)
- Para HTTP: configure Express com middleware e error handling adequados
- Para stdio: use StdioServerTransport diretamente

### Implementacao de Tools
- Use `registerTool()` com nomes descritivos
- Defina schemas usando zod para validacao de input e output
- Forneca campos `title` e `description` claros
- Retorne `content` e `structuredContent` nos resultados
- Implemente error handling com try-catch
- Suporte operacoes async quando apropriado

### Setup de Resource/Prompt (Opcional)
- Adicione resources com `registerResource()` usando ResourceTemplate para URIs dinamicas
- Adicione prompts com `registerPrompt()` com schemas de argumentos
- Considere adicionar completion support para melhor UX

### Qualidade de Codigo
- Use TypeScript para type safety
- Siga padroes async/await de forma consistente
- Implemente cleanup adequado em eventos de fechamento de transport
- Use variaveis de ambiente para configuracao
- Adicione comentarios inline para logica complexa
- Estruture o codigo com separacao clara de responsabilidades

## Tipos de Tool a Considerar
- Processamento e transformacao de dados
- Integracoes com APIs externas
- Operacoes em file system (read, search, analyze)
- Queries em banco de dados
- Analise ou sumarizacao de texto (com sampling)
- Recuperacao de informacoes do sistema

## Opcoes de Configuracao
- **Para servidores HTTP**: 
  - Configuracao de porta via variaveis de ambiente
  - Setup de CORS para clientes browser
  - Gerenciamento de sessao (stateless vs stateful)
  - Protecao contra DNS rebinding para servidores locais
  
- **Para servidores stdio**:
  - Tratamento adequado de stdin/stdout
  - Configuracao baseada em ambiente
  - Gerenciamento de ciclo de vida do processo

## Orientacao de Testes
- Explique como rodar o servidor (`npm start` ou `npx tsx server.ts`)
- Forneca comando do MCP Inspector: `npx @modelcontextprotocol/inspector`
- Para servidores HTTP, inclua URL de conexao: `http://localhost:PORT/mcp`
- Inclua exemplos de invocacao de tools
- Adicione dicas de troubleshooting para problemas comuns

## Recursos Adicionais a Considerar
- Sampling support para tools com LLM
- User input elicitation para workflows interativos
- Registro dinamico de tools com enable/disable
- Notification debouncing para updates em lote
- Resource links para referencias de dados eficientes

Gere um servidor MCP completo e pronto para producao com documentacao abrangente, type safety e error handling.
