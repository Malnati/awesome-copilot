# Referencia de Patterns de Componentes de UI

## Buttons

### Tipos de Button

| Tipo | Finalidade | Tratamento Visual |
| ---- | ------- | ---------------- |
| Primary | Acao principal na pagina | Preenchimento solido, cor da marca |
| Secondary | Acoes de suporte | Contorno ou preenchimento discreto |
| Tertiary | Acoes de baixa enfase | Somente texto, sublinhado opcional |
| Destructive | Acoes de excluir/remover | Cor vermelha, confirmacao obrigatoria |
| Ghost | UI minima, botoes de icone | Transparente, hover sutil |

### Estados de Button

```text
Default    → Estado de repouso, claramente interativo
Hover      → Cursor sobre (desktop): escurecer 10%, sombra sutil
Active     → Sendo pressionado: escurecer 20%, leve reducao de escala
Focus      → Selecionado via teclado: anel de contorno visivel
Disabled   → Indisponivel: opacidade 50%, cursor: not-allowed
Loading    → Processando: spinner substitui ou acompanha o label

```

### Especificacoes de Button

- **Tamanho minimo:** 44×44px (touch target)
- **Padding:** 12-16px horizontal, 8-12px vertical
- **Border radius:** 4-8px (consistent across app)
- **Font weight:** Medium ou Semibold (600-700)
- **Text:** Sentence case, max 2-4 palavras

### Padroes de Label de Button

```text
✓ Save Changes        ✗ Submit
✓ Add to Cart         ✗ Click Here
✓ Create Account      ✗ OK
✓ Download PDF        ✗ Go
✓ Start Free Trial    ✗ Continue

```

---

## Forms

### Diretrizes de Layout de Form

- **Single column preferred:** Reduz carga cognitiva
- **Top-aligned labels:** Tempos de conclusao mais rapidos
- **Logical grouping:** Campos relacionados juntos
- **Smart defaults:** Preencha automaticamente quando possivel

### Anatomia de Input Field

```text
┌─ Label (required) ─────────────────────────┐
│                                            │
│  ┌────────────────────────────────────┐   │
│  │ Placeholder text...                 │   │
│  └────────────────────────────────────┘   │
│  Helper text or error message              │
└────────────────────────────────────────────┘

```

### Estados de Input

| Estado | Borda | Fundo | Adicional |
| ----- | ------ | ---------- | ---------- |
| Padrao | Gray (#D1D5DB) | White | - |
| Focus | Cor primary | White | Sombra/brilho |
| Filled | Gray | White | Checkmark opcional |
| Erro | Red (#EF4444) | Tonalidade vermelha clara | Icone de erro + mensagem |
| Disabled | Light gray | Gray (#F3F4F6) | Texto com 50% de opacidade |

### Timing de Validacao

- **On blur:** Validar quando o usuario sai do campo
- **On change (after error):** Limpar erro conforme o usuario corrige
- **On submit:** Validacao final antes do processamento
- **Never on focus:** Nao mostrar erros antes do usuario digitar

### Guidelines de Mensagens de Erro

```text
✓ "Email address is required"
✓ "Password must be at least 8 characters"
✓ "Please enter a valid phone number (e.g., 555-123-4567)"

✗ "Invalid input"
✗ "Error"
✗ "This field is required" (generic)

```

### Boas Praticas de Form

- Marque campos opcionais, nao obrigatorios (menos asteriscos)
- Mostre requisitos de senha antes que ocorram erros
- Use input masks para dados formatados (phone, date)
- Preserve dados quando houver erros (nao limpe o form)
- Forneca confirmacao clara de sucesso

---

## Navegacao

### Padroes de Navegacao

#### Top Navigation Bar

```text
┌─────────────────────────────────────────────────────┐
│ Logo    Nav Item  Nav Item  Nav Item    [Search] [User] │
└─────────────────────────────────────────────────────┘

```

- **Melhor para:** Sites de marketing, apps simples
- **Maximo de itens:** 5-7 links de primeiro nivel
- **Mobile:** Recolher para menu hamburger

#### Sidebar Navigation

```text
┌────────┬────────────────────────────────┐
│ Logo   │ Content Area                   │
├────────┤                                │
│ Nav 1  │                                │
│ Nav 2  │                                │
│ Nav 3  │                                │
│        │                                │
│ ────── │                                │
│ Nav 4  │                                │
│ Nav 5  │                                │
└────────┴────────────────────────────────┘

```

- **Melhor para:** Dashboards, apps complexas
- **Width:** 200-280px expanded, 64px collapsed
- **Mobile:** Drawer em overlay

#### Bottom Navigation (Mobile)

```text
┌─────────────────────────────────────┐
│           Content Area              │
│                                     │
├─────────────────────────────────────┤
│  🏠    🔍    ➕    💬    👤        │
│ Inicio Buscar Adicionar Chat Perfil │
└─────────────────────────────────────┘

```

- **Maximo de itens:** 3-5 destinos
- **Melhor para:** Secoes principais do app
- **Sempre visivel:** Navegacao persistente

#### Breadcrumbs

```text
Inicio > Produtos > Eletronicos > Fones

```

- **Use para:** Hierarquias profundas (3+ niveis)
- **Pagina atual:** Nao clicavel, estilo diferente
- **Separator:** > ou / ou icone chevron

### Tab Navigation

```text
┌─────────┬─────────┬─────────┬─────────┐
│ Tab 1   │ Tab 2   │ Tab 3   │ Tab 4   │
└─────────┴─────────┴─────────┴─────────┘
│                                       │
│        Area de Conteudo da Tab        │
│                                       │
└───────────────────────────────────────┘

```

- **Maximo de tabs:** 3-5 para clareza
- **Indicador ativo:** Sublinhado ou fundo
- **Use para:** Conteudo relacionado na mesma pagina

---

## Cards

### Card Anatomy

```text
┌─────────────────────────────────┐
│ ░░░░░░░ Imagem/Midia ░░░░░░░░░ │
├─────────────────────────────────┤
│ Label de Categoria              │
│ Titulo do Card                  │
│ Texto de descricao que pode     │
│ ocupar varias linhas...         │
│                                 │
│ [Botao de Acao]  [Secondary]    │
└─────────────────────────────────┘

```

### Guidelines de Card

- **Tamanho consistente:** Use grid, alturas iguais
- **Hierarquia de conteudo:** Image → Title → Description → Actions
- **Padding:** 16-24px de espacamento interno
- **Border radius:** 8-12px (matching buttons)
- **Shadow:** Elevacao sutil (0 2px 4px rgba(0,0,0,0.1))

---

## Modals and Dialogs

### Estrutura de Modal

```text
┌─────────────────────────────────────┐
│ Titulo do Modal                [×]  │
├─────────────────────────────────────┤
│                                     │
│ Conteudo do modal vai aqui.         │
│ Mantenha focado em uma tarefa.      │
│                                     │
├─────────────────────────────────────┤
│           [Cancelar] [Confirmar]    │
└─────────────────────────────────────┘

```

### Guidelines de Modal

- **Tamanho:** 400-600px de largura (desktop), largura total menos margens (mobile)
- **Overlay:** Fundo escuro semitransparente (rgba(0,0,0,0.5))
- **Opcoes de fechamento:** Botao X, clique no overlay, tecla Escape
- **Focus trap:** Mantenha o foco do teclado dentro do modal
- **Acao primaria:** Alinhada a direita, visualmente destacada

---

## Dashboards

### Principios de Layout de Dashboard

1. **Most important metrics at top:** KPIs, cards de resumo
2. **Progressive detail:** Overview → capacidade de drill-down
3. **Tamanhos de card consistentes:** Use sistema de grid
4. **Minimal chartjunk:** Apenas visuais que servem aos dados
5. **Actionable insights:** Destaque anomalias

### Selecao de Visualizacao de Dados

| Tipo de Dado | Tipo de Grafico |
| --------- | ---------- |
| Comparacao entre categorias | Bar chart |
| Tendencia ao longo do tempo | Line chart |
| Parte do todo | Pie (≤5 slices) ou Donut |
| Distribuicao | Histogram |
| Correlacao | Scatter plot |
| Geografico | Map |
| Metrica unica | Big number + sparkline |

### Boas Praticas de Dashboard

- **Limite a 5-9 widgets** por view
- **Alinhe ao grid:** Gutters e tamanhos consistentes
- **Controles de filtro:** Top ou sidebar, sempre visivel
- **Seletor de intervalo de datas:** Necessidade comum, destaque
- **Opcoes de exportacao:** PDF, CSV para tabelas de dados
- **Responsivo:** Empilhe cards em telas menores

---

## Empty States

### Componentes de Empty State

```text
┌─────────────────────────────────────┐
│                                     │
│         [Ilustracao/Icone]          │
│                                     │
│      Nenhum projeto ainda           │
│                                     │
│   Crie seu primeiro projeto para    │
│   comecar a organizar seu trabalho. │
│                                     │
│       [Criar Projeto]               │
│                                     │
└─────────────────────────────────────┘

```

### Guidelines de Empty State

- **Ilustracao amigavel:** Nao apenas "No data"
- **Explique o valor:** Por que criar algo?
- **CTA claro:** Acao primaria para resolver o empty state
- **Seja breve:** 1-2 frases no maximo

---

## Loading States

### Loading Patterns

| Duracao | Padrao |
| -------- | ------- |
| <1 second | Sem indicador (parece instantaneo) |
| 1-3 seconds | Spinner ou indicador de progresso |
| 3-10 seconds | Skeleton screens + progresso |
| >10 seconds | Barra de progresso + explicacao |

### Skeleton Screen

```text
┌─────────────────────────────────────┐
│ ░░░░░░░░░░░░ ░░░░░░░░░░           │
├─────────────────────────────────────┤
│ ░░░░░░░░░░░░░░░░░░░░░░░░░         │
│ ░░░░░░░░░░░░░░░░░░░               │
│ ░░░░░░░░░░░░░░░░░░░░░░░           │
└─────────────────────────────────────┘

```

- Combine com o layout do conteudo carregado
- Use animacao sutil (shimmer/pulse)
- Mostre a estrutura real do conteudo