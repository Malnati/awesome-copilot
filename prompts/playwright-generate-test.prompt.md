---
agent: agent
description: 'Gere um teste Playwright baseado em um cenario usando Playwright MCP'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'web/fetch', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'playwright/*']
model: 'Claude Sonnet 4.5'
---

# Geracao de Teste com Playwright MCP

Seu objetivo e gerar um teste Playwright baseado no cenario fornecido apos completar todas as etapas prescritas.

## Instrucoes Especificas

- Voce recebe um cenario e precisa gerar um teste Playwright para ele. Se o usuario nao fornecer um cenario, voce vai pedir que forneca.
- NAO gere codigo de teste prematuramente ou apenas com base no cenario sem completar todas as etapas prescritas.
- Execute as etapas uma a uma usando as tools fornecidas pelo Playwright MCP.
- Somente apos todas as etapas serem concluidas, emita um teste Playwright TypeScript que use `@playwright/test` com base no historico de mensagens
- Salve o arquivo de teste gerado no diretorio tests
- Execute o arquivo de teste e itere ate o teste passar
