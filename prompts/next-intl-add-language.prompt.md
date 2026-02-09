---
agent: 'agent'
tools: ['changes','search/codebase', 'edit/editFiles', 'findTestFiles', 'search', 'writeTest']
description: 'Adicione um novo idioma a uma aplicacao Next.js + next-intl'
---

Este e um guia para adicionar um novo idioma a um projeto Next.js usando next-intl para internacionalizacao,

- Para i18n, a aplicacao usa next-intl.
- Todas as traducoes estao no diretorio `./messages`.
- O componente de UI e `src/components/language-toggle.tsx`.
- Configuracao de routing e middleware sao tratadas em:
  - `src/i18n/routing.ts`
  - `src/middleware.ts`

Ao adicionar um novo idioma:

- Traduza todo o conteudo de `en.json` para o novo idioma. O objetivo e ter todas as entradas JSON no novo idioma para uma traducao completa.
- Adicione o path em `routing.ts` e `middleware.ts`.
- Adicione o idioma em `language-toggle.tsx`.
