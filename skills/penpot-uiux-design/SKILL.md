---
name: penpot-uiux-design
description: 'Guia abrangente para criar designs profissionais de UI/UX no Penpot usando tools MCP. Use esta skill quando: (1) Criar novos designs de UI/UX para apps web, mobile ou desktop, (2) Construir design systems com componentes e tokens, (3) Projetar dashboards, forms, navegacao ou landing pages, (4) Aplicar padroes de acessibilidade e boas praticas, (5) Seguir diretrizes de plataforma (iOS, Android, Material Design), (6) Revisar ou melhorar designs existentes no Penpot para usabilidade. Gatilhos: "design a UI", "create interface", "build layout", "design dashboard", "create form", "design landing page", "make it accessible", "design system", "component library".'
---

# Guia de Design UI/UX no Penpot

Crie designs profissionais e centrados no usuario no Penpot usando o servidor MCP `penpot/penpot-mcp` e principios comprovados de UI/UX.

## MCP Tools Disponiveis

| Tool | Finalidade |
| ---- | ------- |
| `mcp__penpot__execute_code` | Executar JavaScript no contexto do plugin Penpot para criar/modificar designs |
| `mcp__penpot__export_shape` | Exportar shapes como PNG/SVG para inspecao visual |
| `mcp__penpot__import_image` | Importar imagens (icones, fotos, logos) nos designs |
| `mcp__penpot__penpot_api_info` | Recuperar documentacao da API do Penpot |

## Configuracao do Servidor MCP

As tools MCP do Penpot requerem o servidor `penpot/penpot-mcp` rodando localmente. Para instalacao e troubleshooting detalhados, veja [setup-troubleshooting.md](references/setup-troubleshooting.md).

### Antes da Configuracao: Verifique se Ja Esta Rodando

**Sempre verifique se o servidor MCP ja esta disponivel antes de tentar setup:**

1. **Tente chamar uma tool primeiro**: Tente `mcp__penpot__penpot_api_info` - se funcionar, o servidor esta rodando e conectado. Nao e necessario setup.

2. **Se a tool falhar**, pergunte ao usuario:
   > "O servidor MCP do Penpot nao parece estar conectado. Ele ja esta instalado e rodando? Se sim, posso ajudar a fazer troubleshooting. Se nao, posso guiar o setup."

3. **So prossiga com instrucoes de setup se o usuario confirmar que o servidor nao esta instalado.**

### Inicio Rapido (somente se nao estiver instalado)

```bash
# Clone and install
git clone https://github.com/penpot/penpot-mcp.git
cd penpot-mcp
npm install

# Build and start servers
npm run bootstrap
```

Depois no Penpot:
1. Abra um arquivo de design
2. Va em **Plugins** → **Load plugin from URL**
3. Insira: `http://localhost:4400/manifest.json`
4. Clique em **"Connect to MCP server"** na UI do plugin

### Configuracao do VS Code

Adicione em `settings.json`:
```json
{
  "mcp": {
    "servers": {
      "penpot": {
        "url": "http://localhost:4401/sse"
      }
    }
  }
}
```

### Solucao de Problemas (se o servidor estiver instalado mas nao funcionar)

| Issue | Solution |
| ----- | -------- |
| Plugin nao conecta | Verifique se os servidores estao rodando (`npm run start:all` no dir penpot-mcp) |
| Browser bloqueia localhost | Permita acesso de rede local, desative Brave Shield ou tente Firefox |
| Tools nao aparecem no client | Reinicie completamente VS Code/Claude apos mudancas de config |
| Execucao de tool falha/timeout | Garanta que a UI do plugin Penpot esteja aberta e mostre "Connected" |
| "WebSocket connection failed" | Verifique se o firewall permite portas 4400, 4401, 4402 |

## Referencia Rapida

| Tarefa | Arquivo de Referencia |
| ---- | -------------- |
| Instalacao e troubleshooting do servidor MCP | [setup-troubleshooting.md](references/setup-troubleshooting.md) |
| Specs de componentes (buttons, forms, nav) | [component-patterns.md](references/component-patterns.md) |
| Acessibilidade (contraste, touch targets) | [accessibility.md](references/accessibility.md) |
| Tamanhos de tela e specs de plataforma | [platform-guidelines.md](references/platform-guidelines.md) |

## Principios Centrais de Design

### Regras de Ouro

1. **Clareza acima de esperteza**: Cada elemento deve ter um proposito
2. **Consistencia gera confianca**: Reuse padroes, cores e componentes
3. **Metas do usuario primeiro**: Projete para tarefas, nao para features
4. **Acessibilidade nao e opcional**: Desenhe para todos
5. **Teste com usuarios reais**: Valide suposicoes cedo

### Hierarquia Visual (Ordem de Prioridade)

1. **Tamanho**: Maior = mais importante
2. **Cor/Contraste**: Alto contraste chama atencao
3. **Posicao**: Top-left (LTR) e visto primeiro
4. **Espaco em branco**: Isolamento enfatiza importancia
5. **Peso tipografico**: Bold se destaca

## Workflow de Design

1. **Cheque por design system primeiro**: Pergunte se ha tokens/specs existentes, ou descubra a partir do arquivo do Penpot
2. **Entenda a pagina**: Chame `mcp__penpot__execute_code` com `penpotUtils.shapeStructure()` para ver a hierarquia
3. **Encontre elementos**: Use `penpotUtils.findShapes()` para localizar elementos por tipo ou nome
4. **Criar/modificar**: Use `penpot.createBoard()`, `penpot.createRectangle()`, `penpot.createText()` etc.
5. **Aplicar layout**: Use `addFlexLayout()` para containers responsivos
6. **Validar**: Chame `mcp__penpot__export_shape` para checar visualmente o trabalho

## Tratamento de Design System

**Antes de criar designs, determine se o usuario tem um design system existente:**

1. **Pergunte ao usuario**: "Voce tem um design system ou brand guidelines a seguir?"
2. **Descubra no Penpot**: Verifique componentes, cores e padroes existentes

```javascript
// Discover existing design patterns in current file
const allShapes = penpotUtils.findShapes(() => true, penpot.root);

// Find existing colors in use
const colors = new Set();
allShapes.forEach(s => {
  if (s.fills) s.fills.forEach(f => colors.add(f.fillColor));
});

// Find existing text styles (font sizes, weights)
const textStyles = allShapes
  .filter(s => s.type === 'text')
  .map(s => ({ fontSize: s.fontSize, fontWeight: s.fontWeight }));

// Find existing components
const components = penpot.library.local.components;

return { colors: [...colors], textStyles, componentCount: components.length };
```

**Se o usuario TEM um design system:**

- Use cores, espacamento e tipografia especificados
- Combine com os padroes de componentes existentes
- Siga as convencoes de nomeacao

**Se o usuario NAO tem design system:**

- Use os tokens default abaixo como ponto de partida
- Ofereca ajuda para estabelecer padroes consistentes
- Referencie specs em [component-patterns.md](references/component-patterns.md)

## Gotchas Principais da API do Penpot

- `width`/`height` sao READ-ONLY → use `shape.resize(w, h)`
- `parentX`/`parentY` sao READ-ONLY → use `penpotUtils.setParentXY(shape, x, y)`
- Use `insertChild(index, shape)` para z-order (nao `appendChild`)
- A ordem do array de flex children e REVERSED para `dir="column"` ou `dir="row"`
- Apos `text.resize()`, resete `growType` para "auto-width" ou "auto-height"