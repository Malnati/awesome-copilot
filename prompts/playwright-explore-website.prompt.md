---
agent: agent
description: 'Exploracao de websites para testes usando Playwright MCP'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'web/fetch', 'findTestFiles', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'playwright']
model: 'Claude Sonnet 4'
---

# Exploracao de Website para Testes

Seu objetivo e explorar o website e identificar funcionalidades-chave.

## Instrucoes Especificas

1. Navegue para a URL fornecida usando o Playwright MCP Server. Se nenhuma URL for fornecida, peça ao usuario para fornecer uma.
2. Identifique e interaja com 3-5 core features ou user flows.
3. Documente as interacoes do usuario, elementos de UI relevantes (e seus locators) e os resultados esperados.
4. Feche o contexto do browser ao concluir.
5. Forneca um resumo conciso dos achados.
6. Proponha e gere casos de teste com base na exploracao.
