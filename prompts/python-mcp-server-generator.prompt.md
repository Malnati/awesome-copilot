---
agent: 'agent'
description: 'Gere um projeto completo de servidor MCP em Python com tools, resources e configuracao adequada'
---

# Gerar Servidor MCP em Python

Crie um servidor Model Context Protocol (MCP) completo em Python com as seguintes especificacoes:

## Requisitos

1. **Estrutura do Projeto**: Crie um novo projeto Python com estrutura adequada usando uv
2. **Dependencias**: Inclua o pacote mcp[cli] com uv
3. **Tipo de Transport**: Escolha entre stdio (local) ou streamable-http (remoto)
4. **Tools**: Crie ao menos uma tool util com type hints adequados
5. **Error Handling**: Inclua tratamento de erros e validacao abrangentes

## Detalhes de Implementacao

### Setup do Projeto
- Inicialize com `uv init project-name`
- Adicione o SDK MCP: `uv add "mcp[cli]"`
- Crie o arquivo principal do servidor (por exemplo, `server.py`)
- Adicione `.gitignore` para projetos Python
- Configure para execucao direta com `if __name__ == "__main__"`

### Configuracao do Servidor
- Use a classe `FastMCP` de `mcp.server.fastmcp`
- Defina nome do servidor e instrucoes opcionais
- Escolha transport: stdio (default) ou streamable-http
- Para HTTP: configure host, porta e modo stateless opcionalmente

### Implementacao de Tools
- Use o decorator `@mcp.tool()` nas funcoes
- Sempre inclua type hints - eles geram schemas automaticamente
- Escreva docstrings claras - elas viram descricoes das tools
- Use modelos Pydantic ou TypedDicts para saidas estruturadas
- Suporte operacoes async para tarefas I/O-bound
- Inclua tratamento de erros adequado

### Setup de Resource/Prompt (Opcional)
- Adicione resources com o decorator `@mcp.resource()`
- Use URI templates para resources dinamicos: "resource://{param}"
- Adicione prompts com o decorator `@mcp.prompt()`
- Retorne strings ou listas de Message em prompts

### Qualidade de Codigo
- Use type hints em todos os parametros e retornos
- Escreva docstrings para tools, resources e prompts
- Siga as diretrizes PEP 8
- Use async/await para operacoes assincronas
- Implemente context managers para cleanup de recursos
- Adicione comentarios inline para logica complexa

## Tipos de Tool a Considerar
- Processamento e transformacao de dados
- Operacoes em file system (read, analyze, search)
- Integracoes com APIs externas
- Queries em banco de dados
- Analise ou geracao de texto (com sampling)
- Recuperacao de informacoes do sistema
- Calculos matematicos ou cientificos

## Opcoes de Configuracao
- **Para servidores stdio**:
  - Execucao direta simples
  - Testar com `uv run mcp dev server.py`
  - Instalar no Claude: `uv run mcp install server.py`
  
- **Para servidores HTTP**:
  - Configuracao de porta via variaveis de ambiente
  - Modo stateless para escalabilidade: `stateless_http=True`
  - Modo de resposta JSON: `json_response=True`
  - Configuracao CORS para clientes browser
  - Montagem em servidores ASGI existentes (Starlette/FastAPI)

## Orientacao de Testes
- Explique como rodar o servidor:
  - stdio: `python server.py` ou `uv run server.py`
  - HTTP: `python server.py` e conecte em `http://localhost:PORT/mcp`
- Teste com MCP Inspector: `uv run mcp dev server.py`
- Instale no Claude Desktop: `uv run mcp install server.py`
- Inclua exemplos de invocacao de tools
- Adicione dicas de troubleshooting

## Recursos Adicionais a Considerar
- Uso de Context para logging, progresso e notificacoes
- LLM sampling para tools com IA
- User input elicitation para workflows interativos
- Lifespan management para recursos compartilhados (bancos, conexoes)
- Saida estruturada com modelos Pydantic
- Icons para exibicao em UI
- Manipulacao de imagem com Image class
- Completion support para melhor UX

## Boas Praticas
- Use type hints em tudo - nao sao opcionais
- Retorne dados estruturados quando possivel
- Logue em stderr (ou use Context logging) para evitar poluir stdout
- Limpe recursos corretamente
- Valide inputs cedo
- Forneca mensagens de erro claras
- Teste tools de forma independente antes de integrar com LLM

Gere um servidor MCP completo e pronto para producao com type safety, tratamento de erros adequado e documentacao abrangente.
