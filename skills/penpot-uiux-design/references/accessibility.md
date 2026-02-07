# Referencia de Diretrizes de Acessibilidade (WCAG)

## Checklist Rapido de Conformidade

### Requisitos Nivel AA (Padrao Minimo)

- [ ] Contraste de cor 4.5:1 para texto normal
- [ ] Contraste de cor 3:1 para texto grande (18px+ ou 14px bold)
- [ ] Touch targets minimo de 44×44px
- [ ] Toda funcionalidade disponivel via teclado
- [ ] Indicadores de foco visiveis
- [ ] Nenhum conteudo pisca mais de 3 vezes/segundo
- [ ] Pagina tem titulo descritivo
- [ ] Proposito do link claro pelo texto
- [ ] Inputs de formulario com labels
- [ ] Mensagens de erro descritivas

---

## Cor e Contraste

### Razoes de Contraste

| Elemento | Razao Minima | Aprimorado (AAA) |
| ------- | ------------- | -------------- |
| Body text | 4.5:1 | 7:1 |
| Large text (18px+) | 3:1 | 4.5:1 |
| UI components | 3:1 | - |
| Graphical objects | 3:1 | - |

### Independencia de Cor

Nunca use cor como unico meio de transmitir informacao:

```text
✗ Error fields shown only in red
✓ Error fields with red border + error icon + text message

✗ Required fields marked only with red asterisk
✓ Required fields labeled "(required)" or with icon + tooltip

✗ Status shown only by color dots
✓ Status with color + icon + label text

```

### Combinacoes de Cores Acessiveis

**Cores seguras de texto em fundos:**

| Fundo | Cor do Texto | Contraste |
| ---------- | ---------- | -------- |
| White (#FFFFFF) | Dark gray (#1F2937) | 15.5:1 ✓ |
| Light gray (#F3F4F6) | Dark gray (#374151) | 10.9:1 ✓ |
| Primary blue (#2563EB) | White (#FFFFFF) | 4.6:1 ✓ |
| Dark (#111827) | White (#FFFFFF) | 18.1:1 ✓ |

**Cores a evitar para texto:**

- Amarelo no branco (contraste insuficiente)
- Cinza claro no branco
- Laranja no branco (marginal)

---

## Navegacao por Teclado

### Requisitos

1. **Todos os elementos interativos** devem ser alcancaveis pela tecla Tab
2. **Logical tab order** following visual layout
3. **No keyboard traps** (user can always Tab away)
4. **Focus visible** at all times during keyboard navigation
5. **Skip links** to bypass repetitive navigation

### Indicadores de Foco

```css
/* Example focus styles */
:focus {
  outline: 2px solid #2563EB;
  outline-offset: 2px;
}

:focus:not(:focus-visible) {
  outline: none; /* Hide for mouse users */
}

:focus-visible {
  outline: 2px solid #2563EB;
  outline-offset: 2px;
}

```

### Atalhos de Teclado

| Tecla | Comportamento Esperado |
| --- | ----------------- |
| Tab | Ir para o proximo elemento interativo |
| Shift+Tab | Ir para o elemento anterior |
| Enter | Ativar botao/link |
| Space | Ativar botao, alternar checkbox |
| Escape | Fechar modal/dropdown |
| Arrow keys | Navegar dentro de componentes |

---

## Suporte a Leitor de Tela

### Elementos HTML Semanticos

Use elementos apropriados para cada proposito:

| Finalidade | Elemento | Nao Isto |
| ------- | ------- | -------- |
| Navigation | `<nav>` | `<div class="nav">` |
| Main content | `<main>` | `<div id="main">` |
| Header | `<header>` | `<div class="header">` |
| Footer | `<footer>` | `<div class="footer">` |
| Button | `<button>` | `<div onclick>` |
| Link | `<a href>` | `<span onclick>` |

### Hierarquia de Titulos

```text
h1 - Titulo da Pagina (um por pagina)
  h2 - Secao Principal
    h3 - Subsecao
      h4 - Sub-subsecao
    h3 - Outra Subsecao
  h2 - Outra Secao Principal

```

**Nunca pule niveis** (h1 → h3 sem h2)

### Texto Alt de Imagens

```text
Decorativa: alt="" (vazio, nao omitido)
Informativa: alt="Descricao do que a imagem mostra"
Funcional: alt="Acao que a imagem executa"
Complexa: alt="Descricao breve" + descricao detalhada proxima

```

**Exemplos de texto alt:**

```text
✓ alt="Bar chart showing sales growth from $10M to $15M in Q4"
✓ alt="Company logo"
✓ alt="" (for decorative background pattern)

✗ alt="image" or alt="photo"
✗ alt="img_12345.jpg"
✗ Missing alt attribute entirely

```

---

## Toque e Ponteiro

### Tamanhos de Alvo de Toque

| Plataforma | Minimo | Recomendado |
| -------- | ------- | ----------- |
| WCAG 2.1 | 44×44px | 48×48px |
| iOS (Apple) | 44×44pt | - |
| Android | 48×48dp | - |

### Espacamento entre Alvos de Toque

- Minimo de 8px entre alvos adjacentes
- Prefira 16px+ para conforto
- Alvos maiores para acoes primarias

### Gestos de Ponteiro

- Gestos complexos devem ter alternativas de ponteiro unico
- Operacoes de arrastar precisam de acoes equivalentes por clique
- Evite funcionalidade apenas por hover em dispositivos touch

---

## Acessibilidade de Formularios

### Labels

Todo input deve ter um label associado:

```text
<label for="email">Email Address</label>
<input type="email" id="email" name="email">

```

### Campos Obrigatorios

```text
<!-- Announce to screen readers -->
<label for="name">
  Name <span aria-label="required">*</span>
</label>
<input type="text" id="name" required aria-required="true">

```

### Tratamento de Erros

```text
<label for="email">Email</label>
<input type="email" id="email" aria-invalid="true" aria-describedby="email-error">
<span id="email-error" role="alert">
  Please enter a valid email address
</span>

```

### Instrucoes de Formulario

- Forneca dicas de formato antes do input
- Mostre requisitos de senha antes dos erros
- Agrupe campos relacionados com fieldset/legend

---

## Conteudo Dinamico

### Live Regions

Para conteudo que atualiza dinamicamente:

```text
aria-live="polite" - Announce when convenient
aria-live="assertive" - Announce immediately (interrupts)
role="alert" - Urgent messages (like assertive)
role="status" - Status updates (like polite)

```

### Estados de Carregamento

```text
<button aria-busy="true" aria-live="polite">
  <span class="spinner"></span>
  Loading...
</button>

```

### Dialogs Modais

- O foco vai para o modal quando aberto
- Foco preso dentro do modal
- A tecla Escape fecha o modal
- O foco volta ao elemento que disparou ao fechar

---

## Teste de Acessibilidade

### Checklist de Teste Manual

1. **Somente teclado:** Navegue toda a pagina com Tab/Enter
2. **Leitor de tela:** Teste com VoiceOver (Mac) ou NVDA (Windows)
3. **Zoom 200%:** Conteudo permanece legivel e utilizavel
4. **Alto contraste:** Teste com modo de alto contraste do sistema
5. **Sem mouse:** Conclua todas as tarefas sem dispositivo de apontamento

### Ferramentas Automatizadas

- axe DevTools (extensao de navegador)
- WAVE (extensao de navegador WebAIM)
- Lighthouse (Chrome DevTools)
- Verificadores de contraste de cor (WebAIM, Contrast Ratio)

### Problemas Comuns para Verificar

- [ ] Texto alt ausente ou vazio
- [ ] Links ou botoes vazios
- [ ] Labels de formulario ausentes
- [ ] Contraste de cor insuficiente
- [ ] Atributo de idioma ausente
- [ ] Estrutura incorreta de titulos
- [ ] Link de pular navegacao ausente
- [ ] Widgets customizados inacessiveis

---

## Referencia Rapida de ARIA

### Roles

| Role | Finalidade |
| ---- | ------- |
| `button` | Botao clicavel |
| `link` | Link de navegacao |
| `dialog` | Dialog de modal |
| `alert` | Mensagem importante |
| `navigation` | Regiao de navegacao |
| `main` | Conteudo principal |
| `search` | Funcionalidade de busca |
| `tab/tablist/tabpanel` | Interface de tabs |

### Properties

| Propriedade | Finalidade |
| -------- | ------- |
| `aria-label` | Nome acessivel |
| `aria-labelledby` | Referencia ao elemento de label |
| `aria-describedby` | Referencia a descricao |
| `aria-hidden` | Ocultar de tecnologia assistiva |
| `aria-expanded` | Estado expansivel |
| `aria-selected` | Estado de selecao |
| `aria-disabled` | Estado desabilitado |
| `aria-required` | Campo obrigatorio |
| `aria-invalid` | Input invalido |

### Regra de Ouro

**Primeira regra do ARIA:** Nao use ARIA se HTML nativo funcionar.

```text
✗ <div role="button" tabindex="0">Click</div>
✓ <button>Click</button>

```