# Referencia de Diretrizes de Design por Plataforma

## Fundamentos de Design Mobile

### Tamanhos de Tela

| Dispositivo | Tamanho | Design em |
| ------ | ---- | --------- |
| iPhone SE | 375×667 | Mobile pequeno |
| iPhone 14/15 | 390×844 | Mobile padrao |
| iPhone 14 Pro Max | 430×932 | Mobile grande |
| Android small | 360×640 | Alvo minimo |
| Android large | 412×915 | Android grande |

### Safe Areas (Areas Seguras)

```text
┌─────────────────────────────────┐
│ ▓▓▓▓▓▓▓ Barra de Status ▓▓▓▓▓▓ │ 44-47px
├─────────────────────────────────┤
│                                 │
│      Area Segura de Conteudo    │
│                                 │
│                                 │
├─────────────────────────────────┤
│ ▓▓▓▓▓▓ Indicador de Home ▓▓▓▓▓ │ 34px
└─────────────────────────────────┘

```

### Touch Targets (Alvos de Toque)

- **Minimo:** 44×44pt (iOS) / 48×48dp (Android)
- **Recomendado:** 48×48px para todas as plataformas
- **Espacamento:** Minimo de 8px entre targets

---

## iOS Human Interface Guidelines (HIG)

### Filosofia de Design

- **Clareza:** Texto legivel, icones precisos, adornos sutis
- **Deferencia:** A UI ajuda as pessoas a entender o conteudo, nunca compete
- **Profundidade:** Camadas visuais distintas transmitem hierarquia

### Padroes de Navegacao

| Padrao | Quando Usar |
| ------- | ----------- |
| Tab Bar | 3-5 destinos de primeiro nivel |
| Navigation Bar | Conteudo hierarquico |
| Sidebar | iPad, apps com conteudo rico |
| Search | Descoberta de conteudo |

### Especificacoes da Tab Bar

```text
┌─────────────────────────────────┐
│  🏠    🔍    ➕    💬    👤    │
│ Inicio Buscar Add  Chat  Perfil │ 49pt height
└─────────────────────────────────┘

```

- Maximo de 5 tabs
- Icones 25×25pt com labels 10pt
- Tab ativa usa cor de fill/tint
- Tabs inativas usam cinza

### Navigation Bar (Barra de Navegacao)

```text
┌─────────────────────────────────┐
│ ‹ Voltar  Titulo da Pagina  Acao │ 44pt minimo
└─────────────────────────────────┘

```

- Esquerda: Botao de voltar ou cancelar
- Centro: Titulo
- Direita: Acao primaria (texto ou icone)

### Tipografia (SF Pro)

| Estilo | Tamanho | Peso |
| ----- | ---- | ------ |
| Large Title | 34pt | Bold |
| Title 1 | 28pt | Bold |
| Title 2 | 22pt | Bold |
| Title 3 | 20pt | Semibold |
| Headline | 17pt | Semibold |
| Body | 17pt | Regular |
| Callout | 16pt | Regular |
| Subhead | 15pt | Regular |
| Footnote | 13pt | Regular |
| Caption | 12pt | Regular |

### Cores iOS (System)

| Cor | Claro | Escuro |
| ----- | ----- | ---- |
| Label | #000000 | #FFFFFF |
| Secondary Label | #3C3C43 @ 60% | #EBEBF5 @ 60% |
| Tertiary Label | #3C3C43 @ 30% | #EBEBF5 @ 30% |
| System Blue | #007AFF | #0A84FF |
| System Green | #34C759 | #30D158 |
| System Red | #FF3B30 | #FF453A |
| System Orange | #FF9500 | #FF9F0A |

### Padroes Especificos de iOS

- **Gestos de swipe:** Delete, archive, actions
- **Pull to refresh:** Standard list refresh
- **Long press:** Context menus
- **Haptic feedback:** Confirm actions
- **Edge swipe:** Back navigation

---

## Android Material Design

### Filosofia de Design Android

- **Material as metaphor:** Propriedades fisicas, elevacao
- **Bold, graphic, intentional:** Cor, tipografia e espaco deliberados
- **Motion provides meaning:** Feedback e continuidade

### Padroes de Navegacao Android

| Padrao | Quando Usar |
| ------- | ----------- |
| Bottom Navigation | 3-5 destinos principais |
| Navigation Drawer | 5+ destinos, menos frequentes |
| Navigation Rail | Tablet em landscape |
| Tabs | Grupos de conteudo relacionados |

### Bottom Navigation

```text
┌─────────────────────────────────┐
│  🏠    🔍    📷    💬    👤    │
│ Inicio Buscar Camera Chat Conta │ 80dp height
└─────────────────────────────────┘

```

- 3-5 destinos
- Icones 24dp com labels 12sp
- Ativo: icone preenchido + cor primaria
- Inativo: icone contornado + on-surface

### App Bar

```text
┌─────────────────────────────────┐
│ ≡  App Title                🔍 │ 64dp height
└─────────────────────────────────┘

```

- Esquerda: icone de navegacao (menu ou voltar)
- Centro: Titulo (pode ser alinhado a esquerda)
- Direita: icones de acao (max 3)

### Floating Action Button (FAB)

- **Tamanho:** 56dp padrao, 40dp mini
- **Posicao:** Canto inferior direito, 16dp das bordas
- **Proposito:** Somente acao primaria
- **Comportamento:** Pode ocultar ao rolar

### Tipografia (Roboto)

| Estilo | Tamanho | Peso | Espacamento |
| ----- | ---- | ------ | -------- |
| Display Large | 57sp | Regular | -0.25 |
| Display Medium | 45sp | Regular | 0 |
| Display Small | 36sp | Regular | 0 |
| Headline Large | 32sp | Regular | 0 |
| Headline Medium | 28sp | Regular | 0 |
| Headline Small | 24sp | Regular | 0 |
| Title Large | 22sp | Regular | 0 |
| Title Medium | 16sp | Medium | 0.15 |
| Title Small | 14sp | Medium | 0.1 |
| Body Large | 16sp | Regular | 0.5 |
| Body Medium | 14sp | Regular | 0.25 |
| Body Small | 12sp | Regular | 0.4 |
| Label Large | 14sp | Medium | 0.1 |
| Label Medium | 12sp | Medium | 0.5 |
| Label Small | 11sp | Medium | 0.5 |

### Cores do Material

| Papel | Finalidade |
| ---- | ------- |
| Primary | Cor principal da marca |
| On Primary | Texto/icones sobre primary |
| Primary Container | Botoes preenchidos, estados ativos |
| Secondary | Componentes menos proeminentes |