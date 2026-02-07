# Visual Inspection Checklist

Este documento e um checklist abrangente de itens para verificar durante a inspecao visual de design web.

---

## 1. Verificacao de Layout

### Integridade Estrutural

- [ ] Header esta corretamente fixo/posicionado no topo da tela
- [ ] Footer esta posicionado no final da tela ou do conteudo
- [ ] Area principal esta centralizada com largura apropriada
- [ ] Sidebar (se houver) esta posicionada corretamente
- [ ] Navegacao e exibida na posicao esperada

### Overflow

- [ ] Barra de rolagem horizontal nao aparece de forma nao intencional
- [ ] Conteudo nao transborda dos elementos pais
- [ ] Imagens cabem dentro dos containers
- [ ] Tabelas nao excedem a largura do container
- [ ] Blocos de codigo quebram ou rolam adequadamente

### Alinhamento

- [ ] Itens em grid distribuem de forma uniforme
- [ ] Alinhamento de itens em flex esta correto
- [ ] Alinhamento de texto (left/center/right) e consistente
- [ ] Icones e texto estao alinhados verticalmente
- [ ] Labels de formularios e campos de entrada estao posicionados corretamente

---

## 2. Verificacao de Tipografia

### Legibilidade

- [ ] Tamanho do texto do corpo e suficiente (minimo 16px recomendado)
- [ ] Altura de linha adequada (1.5-1.8 recomendado)
- [ ] Numero de caracteres por linha e adequado (40-80 recomendado)
- [ ] Espacamento entre paragrafos e suficiente
- [ ] Hierarquia de tamanhos de titulos e clara

### Tratamento de Texto

- [ ] Palavras longas quebram adequadamente
- [ ] URLs e codigo sao tratados corretamente
- [ ] Nao ocorre text clipping
- [ ] Ellipsis (...) exibe corretamente
- [ ] Regras de quebra de linha especificas do idioma funcionam

### Fontes

- [ ] Web fonts carregam corretamente
- [ ] Fallback fonts sao apropriadas
- [ ] Pesos de fonte estao conforme esperado
- [ ] Caracteres especiais e emoji exibem corretamente

---

## 3. Verificacao de Cor e Contraste

### Acessibilidade (WCAG Standards)

- [ ] Texto do corpo: contraste 4.5:1 ou maior (AA)
- [ ] Texto grande (18px+ bold ou 24px+): 3:1 ou maior
- [ ] Bordas de elementos interativos: 3:1 ou maior
- [ ] Indicadores de foco: contraste suficiente com o fundo

### Consistencia de Cores

- [ ] Cores de marca estao unificadas
- [ ] Cores de link sao consistentes
- [ ] Vermelho de erro e unificado
- [ ] Verde de sucesso e unificado
- [ ] Cores de hover/active sao apropriadas

### Diversidade de Visao de Cores

- [ ] Informacao transmitida por forma e texto, nao apenas por cor
- [ ] Graficos e diagramas consideram diversidade de visao de cores
- [ ] Mensagens de erro nao dependem apenas de cor

---

## 4. Verificacao de Responsividade

### Mobile (~640px)

- [ ] Conteudo cabe dentro da largura da tela
- [ ] Touch targets tem 44x44px ou mais
- [ ] Texto esta legivel
- [ ] Nao ocorre rolagem horizontal
- [ ] Navegacao e mobile-friendly (hamburger menu, etc.)
- [ ] Inputs de formulario sao faceis de usar

### Tablet (641px~1024px)

- [ ] Layout esta otimizado para tablet
- [ ] Layouts de duas colunas exibem corretamente
- [ ] Tamanhos de imagem sao apropriados
- [ ] Exibicao/ocultacao de sidebar e apropriada

### Desktop (1025px~)

- [ ] Largura maxima esta definida e nao quebra em telas muito grandes
- [ ] Espacamento e suficiente
- [ ] Layouts multi-coluna funcionam corretamente
- [ ] Estados de hover estao implementados

### Transicoes de Breakpoint

- [ ] Layout transita suavemente ao mudar o tamanho da tela
- [ ] Layout nao quebra em tamanhos intermediarios
- [ ] Nenhum conteudo desaparece ou duplica

---

## 5. Verificacao de Elementos Interativos

### Botoes

- [ ] Estado default e claro
- [ ] Estado de hover existe (desktop)
- [ ] Estado de foco e visualmente claro
- [ ] Estado ativo (pressed) existe
- [ ] Estado desabilitado e distinguivel
- [ ] Estado de loading (se aplicavel)

### Links

- [ ] Links sao visualmente identificaveis
- [ ] Links visitados sao distinguiveis (se necessario)
- [ ] Estado de hover existe
- [ ] Estado de foco e claro

### Elementos de Formulario

- [ ] Limites do campo de input sao claros
- [ ] Contraste do placeholder e apropriado
- [ ] Feedback visual no foco
- [ ] Exibicao do estado de erro
- [ ] Indicacao de campo obrigatorio
- [ ] Dropdowns funcionam corretamente

---

## 6. Verificacao de Imagens e Midia

### Imagens

- [ ] Imagens exibem no tamanho adequado
- [ ] Proporcao e mantida
- [ ] Suporte a alta resolucao (@2x)
- [ ] Exibicao quando a imagem falha
- [ ] Lazy loading funciona

### Video e Embeds

- [ ] Videos cabem nos containers
- [ ] Proporcao e mantida
- [ ] Conteudo incorporado e responsivo
- [ ] iframes nao transbordam

---

## 7. Verificacao de Acessibilidade

### Navegacao por Teclado

- [ ] Todos os elementos interativos acessiveis via Tab
- [ ] Ordem de foco e logica
- [ ] Focus traps sao apropriados (modals, etc.)
- [ ] Link de pular para conteudo existe

### Suporte a Leitor de Tela

- [ ] Imagens possuem alt text
- [ ] Formularios possuem labels
- [ ] ARIA labels estao configurados adequadamente
- [ ] Hierarquia de titulos correta (h1→h2→h3...)

### Movimento

- [ ] Animacoes nao sao excessivas
- [ ] prefers-reduced-motion e suportado (se possivel)

---

## 8. Problemas Visuais Relacionados a Performance

### Loading

- [ ] Font FOUT/FOIT e minimo
- [ ] Nao ocorre layout shift (CLS)
- [ ] Nao ha saltos quando imagens carregam
- [ ] Skeleton screens sao apropriados (se aplicavel)

### Animacao

- [ ] Animacoes sao suaves (60fps)
- [ ] Sem problemas de performance ao rolar
- [ ] Transicoes sao naturais

---

## Matriz de Prioridade

| Prioridade | Categoria | Exemplos |
|------------|----------|----------|
| P0 (Critico) | Quebra de funcionalidade | Sobreposicao total, desaparecimento de conteudo |
| P1 (Alta) | Problemas serios de UX | Texto ilegivel, botoes inoperantes |
| P2 (Media) | Problemas moderados | Problemas de alinhamento, inconsistencias de espacamento |
| P3 (Baixa) | Problemas menores | Diferencas leves de posicionamento, variacoes menores de cor |

---

## Ferramentas de Verificacao

### Browser DevTools

- Elements panel: inspecao de DOM e estilos
- Lighthouse: auditorias de performance e acessibilidade
- Device toolbar: teste responsivo

### Ferramentas de Acessibilidade

- axe DevTools
- WAVE
- Color Contrast Analyzer

### Ferramentas de Automacao

- Playwright (comparacao de screenshots)
- Percy / Chromatic (Visual Regression Testing)
