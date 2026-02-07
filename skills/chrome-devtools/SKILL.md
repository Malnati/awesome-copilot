---
name: chrome-devtools
description: 'Automacao de navegador, debugging e analise de performance em nivel especialista usando Chrome DevTools MCP. Use para interagir com paginas web, capturar screenshots, analisar trafego de rede e perfilar performance.'
license: MIT
---

# Agente Chrome DevTools

## Visao Geral

Uma skill especializada para controlar e inspecionar um navegador Chrome ativo. Esta skill utiliza o servidor MCP `chrome-devtools` para executar uma ampla gama de tarefas relacionadas ao navegador, desde navegacao simples ate profiling complexo de performance.

## Quando Usar

Use esta skill quando:

- **Automacao de Navegador**: Navegar em paginas, clicar elementos, preencher formularios e lidar com dialogs.
- **Inspecao Visual**: Tirar screenshots ou snapshots de texto de paginas web.
- **Debugging**: Inspecionar mensagens do console, avaliar JavaScript no contexto da pagina e analisar requests de rede.
- **Analise de Performance**: Registrar e analisar traces de performance para identificar gargalos e problemas de Core Web Vitals.
- **Emulacao**: Redimensionar o viewport ou emular condicoes de rede/CPU.

## Categorias de Tools

### 1. Navegacao e Gerenciamento de Pagina

- `new_page`: Abrir uma nova aba/pagina.
- `navigate_page`: Ir para uma URL especifica, recarregar ou navegar no historico.
- `select_page`: Alternar contexto entre paginas abertas.
- `list_pages`: Ver todas as paginas abertas e seus IDs.
- `close_page`: Fechar uma pagina especifica.
- `wait_for`: Esperar texto especifico aparecer na pagina.

### 2. Entrada e Interacao

- `click`: Clicar em um elemento (use `uid` do snapshot).
- `fill` / `fill_form`: Digitar texto em inputs ou preencher multiplos campos de uma vez.
- `hover`: Mover o mouse sobre um elemento.
- `press_key`: Enviar atalhos de teclado ou teclas especiais (ex.: "Enter", "Control+C").
- `drag`: Arrastar e soltar elementos.
- `handle_dialog`: Aceitar ou dispensar alerts/prompts do navegador.
- `upload_file`: Fazer upload de arquivo via input file.

### 3. Debugging e Inspecao

- `take_snapshot`: Obter uma arvore de acessibilidade baseada em texto (melhor para identificar elementos).
- `take_screenshot`: Capturar representacao visual da pagina ou de um elemento especifico.
- `list_console_messages` / `get_console_message`: Inspecionar a saida do console da pagina.
- `evaluate_script`: Executar JavaScript customizado no contexto da pagina.
- `list_network_requests` / `get_network_request`: Analisar trafego de rede e detalhes de requests.

### 4. Emulacao e Performance

- `resize_page`: Alterar as dimensoes do viewport.
- `emulate`: Throttling de CPU/Network ou emulacao de geolocalizacao.
- `performance_start_trace`: Iniciar gravacao de perfil de performance.
- `performance_stop_trace`: Parar gravacao e salvar o trace.
- `performance_analyze_insight`: Obter analise detalhada dos dados de performance gravados.

## Padroes de Workflow

### Padrao A: Identificar Elementos (Snapshot-First)

Sempre prefira `take_snapshot` em vez de `take_screenshot` para encontrar elementos. O snapshot fornece valores `uid` exigidos pelas tools de interacao.

```markdown
1. `take_snapshot` para obter a estrutura atual da pagina.
2. Encontre o `uid` do elemento alvo.
3. Use `click(uid=...)` ou `fill(uid=..., value=...)`.
```

### Padrao B: Solucao de Problemas de Erros

Quando uma pagina esta falhando, verifique logs do console e requests de rede.

```markdown
1. `list_console_messages` para checar erros de JavaScript.
2. `list_network_requests` para identificar recursos com falha (4xx/5xx).
3. `evaluate_script` para checar o valor de elementos DOM especificos ou variaveis globais.
```

### Padrao C: Profiling de Performance

Identifique por que uma pagina esta lenta.

```markdown
1. `performance_start_trace(reload=true, autoStop=true)`
2. Aguarde a pagina carregar/trace finalizar.
3. `performance_analyze_insight` para encontrar problemas de LCP ou layout shifts.
```

## Boas Praticas

- **Consistencia de Contexto**: Sempre execute `list_pages` e `select_page` se nao tiver certeza de qual aba esta ativa.
- **Snapshots**: Tire um novo snapshot apos qualquer navegacao importante ou mudanca no DOM, pois valores `uid` podem mudar.
- **Timeouts**: Use timeouts razoaveis para `wait_for` para evitar travar em elementos lentos.
- **Screenshots**: Use `take_screenshot` com parcimonia para verificacao visual, mas confie em `take_snapshot` para logica.
