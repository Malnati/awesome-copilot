---
name: webapp-testing
description: Toolkit para interagir e testar aplicacoes web locais usando Playwright. Suporta verificacao de funcionalidade de frontend, depuracao de comportamento de UI, captura de screenshots do navegador e visualizacao de logs do browser.
---

# Web Application Testing

Esta skill permite testes e depuracao abrangentes de aplicacoes web locais usando automacao com Playwright.

## Quando Usar Esta Skill

Use esta skill quando precisar:
- Testar funcionalidades de frontend em um navegador real
- Verificar comportamento de UI e interacoes
- Depurar problemas de aplicacoes web
- Capturar screenshots para documentacao ou depuracao
- Inspecionar logs do console do navegador
- Validar envios de formulario e fluxos do usuario
- Verificar design responsivo em diferentes viewports

## Pre-requisitos

- Node.js instalado no sistema
- Aplicacao web local em execucao (ou URL acessivel)
- Playwright sera instalado automaticamente se nao estiver presente

## Capacidades Principais

### 1. Automacao de Navegador
- Navegar para URLs
- Clicar em botoes e links
- Preencher campos de formulario
- Selecionar dropdowns
- Lidar com dialogs e alerts

### 2. Verificacao
- Confirmar presenca de elementos
- Verificar conteudo de texto
- Checar visibilidade de elementos
- Validar URLs
- Testar comportamento responsivo

### 3. Depuracao
- Capturar screenshots
- Visualizar logs do console
- Inspecionar requisicoes de rede
- Depurar testes que falharam

## Exemplos de Uso

### Exemplo 1: Teste Basico de Navegacao
```javascript
// Navigate to a page and verify title
await page.goto('http://localhost:3000');
const title = await page.title();
console.log('Page title:', title);
```

### Exemplo 2: Interacao com Formulario
```javascript
// Fill out and submit a form
await page.fill('#username', 'testuser');
await page.fill('#password', 'password123');
await page.click('button[type="submit"]');
await page.waitForURL('**/dashboard');
```

### Exemplo 3: Captura de Screenshot
```javascript
// Capture a screenshot for debugging
await page.screenshot({ path: 'debug.png', fullPage: true });
```

## Diretrizes

1. **Sempre verifique se a app esta em execucao** - confira se o servidor local esta acessivel antes de rodar testes
2. **Use esperas explicitas** - aguarde elementos ou navegacao antes de interagir
3. **Capture screenshots em falhas** - tire screenshots para ajudar na depuracao
4. **Libere recursos** - sempre feche o navegador ao finalizar
5. **Trate timeouts com cuidado** - defina timeouts razoaveis para operacoes lentas
6. **Teste incrementalmente** - comece com interacoes simples antes de fluxos complexos
7. **Use seletores de forma inteligente** - prefira data-testid ou seletores por role em vez de classes CSS

## Padroes Comuns

### Padrao: Esperar por Elemento
```javascript
await page.waitForSelector('#element-id', { state: 'visible' });
```

### Padrao: Verificar se Elemento Existe
```javascript
const exists = await page.locator('#element-id').count() > 0;
```

### Padrao: Obter Logs do Console
```javascript
page.on('console', msg => console.log('Browser log:', msg.text()));
```

### Padrao: Tratar Erros
```javascript
try {
  await page.click('#button');
} catch (error) {
  await page.screenshot({ path: 'error.png' });
  throw error;
}
```

## Limitacoes

- Requer ambiente Node.js
- Nao testa apps mobile nativas (use React Native Testing Library)
- Pode ter problemas com fluxos de autenticacao complexos
- Alguns frameworks modernos podem exigir configuracoes especificas
