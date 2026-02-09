---
name: meeting-minutes
description: 'Gere atas de reuniao concisas e acionaveis para reunioes internas. Inclui metadados, participantes, agenda, decisoes, itens de acao (owner + data limite) e passos de follow-up.'
---

# Skill de Atas de Reuniao — Reunioes Internas Curtas

## Proposito / Visao Geral

Esta skill produz atas de reuniao de alta qualidade e consistencia para reunioes internas de 60 minutos ou menos. A saida e clara, acionavel e facil de converter em rastreadores de tarefas (ex.: GitHub Issues, Jira). As atas geradas priorizam decisoes e itens de acao para que os times avancem rapidamente da discussao para execucao.

## Quando Usar

Use esta skill quando:

- Sincronizacoes internas (syncs), standups, revisoes de design (design reviews), triage, planning ou reunioes ad-hoc de curta duracao
- Situacoes que exigem um registro conciso de decisoes, itens de acao atribuídos e follow-ups
- Criar um documento padronizado de atas a partir de reuniao ao vivo, transcricao, gravacao ou notas

---

## Fluxo de Trabalho Operacional

### Fase 1: Coleta (Intake) (antes de redigir)

- Obtenha metadados da reuniao: titulo, data, hora inicio/fim (ou duracao), organizador e publico-alvo.
- Confirme entradas disponiveis: agenda, slides, gravacao, transcricao ou notas brutas.
- Se detalhes-chave estiverem faltando, faca ate 3 perguntas de esclarecimento antes de produzir as atas (ver "Discovery" abaixo).

### Fase 2: Captura (durante / imediatamente apos a reuniao)

- Registre participantes e ausentes.
- Capture notas breves por item de agenda com marcadores de tempo se disponiveis.
- Registre decisoes explicitas, resumo de racional (1-2 frases) e itens de acao (owner + data limite).

### Fase 3: Redacao

- Gere as atas seguindo o **Schema Estrito de Atas** (abaixo).
- Garanta que cada item de acao inclua owner, data limite (ou prazo) e criterios de aceitacao quando aplicavel.
- Marque issues nao resolvidas ou itens que exigem follow-up no Parking Lot.

### Fase 4: Revisao e Publicacao

- Se possivel, envie o draft ao organizador ou revisor designado para verificacao rapida (em ate 24 horas).
- Publique as atas finais no canal combinado (drive compartilhado, repo, ticket ou email) e opcionalmente crie tarefas no tracker do time.

---

## Descoberta (Discovery) (perguntas de esclarecimento obrigatorias)

Antes de gerar as atas, o agente **DEVE** fazer ate tres perguntas de esclarecimento se algum destes itens estiver faltando:

- Qual e o titulo da reuniao, data, hora de inicio (ou duracao) e organizador?
- Ha agenda ou transcricao/gravacao para referencia? Se sim, forneca.
- Quem deve ser o revisor ou aprovador das atas?

Se o usuario responder "sem transcricao" ou "sem agenda", prossiga mas marque a fonte como "notas ad-hoc" e sinalize possiveis lacunas.

---

## Schema Estrito de Atas (Estrutura de Saida)

Voce **DEVE** produzir atas seguindo esta estrutura exata. Se alguma informacao estiver indisponivel, use `TBD` ou `Unknown` e explique como obter.

### 1. Metadados

- **Titulo**:
- **Data (YYYY-MM-DD)**:
- **Hora Inicio (UTC)**:
- **Hora Fim (UTC) ou Duracao**:
- **Organizador**:
- **Local / Link Virtual**:
- **Autor da Ata** (agente ou pessoa):
- **Lista de Distribuicao** (quem recebe as atas):

### 2. Presenca

- **Presentes**: [lista de nomes + roles]
- **Ausentes / Regrets**: [lista]
- **Notetaker / Recorder**: [nome ou "agente"]

### 3. Agenda

Lista de itens da agenda, em ordem:

- Item 1: titulo curto
- Item 2: titulo curto
- ...

### 4. Resumo

Um paragrafo conciso (1-3 frases) com o objetivo da reuniao e o resultado de alto nivel.

### 5. Decisoes Tomadas

Cada decisao como bullet separado:

- **Decisao 1**: declaracao da decisao.
  - Quem decidiu / aprovou: [nome(s) ou grupo]
  - Racional (1-2 frases): motivo breve.
  - Data efetiva (se aplicavel): YYYY-MM-DD
- **Decisao 2**: ...

### 6. Itens de Acao

Bullets estilo tabela; **devem incluir owner e data limite**:

- **[ID] Acao**: descricao curta
  - **Owner**: Nome (time)
  - **Due**: YYYY-MM-DD ou "ASAP" / prazo
  - **Criterios de Aceitacao**: (o que conclui esta acao)
  - **Artefatos / tickets vinculados**: (URL opcional ou id)

**Exemplo:**

- [A1] Draft deployment runbook for feature X
  - Owner: Alex (Engineering)
  - Due: 2026-02-05
  - Acceptance Criteria: runbook includes steps for rollback, health checks, and monitoring links
  - Linked artifacts: https://github.com/owner/repo/issues/123

### 7. Notas por Item de Agenda

Breve, factual, timestamp opcional:

- **Agenda Item 1**: titulo
  - Key points:
    - Point A (timestamp 00:05)
    - Point B (timestamp 00:12)
  - Open issues / questions:
    - Q1: texto da pergunta (owner se atribuido)
- **Agenda Item 2**: ...

### 8. Parking Lot (Itens Nao Resolvidos)

- **Item**: descricao curta
  - Por que foi estacionado / proximo passo:
  - Owner sugerido ou proxima reuniao para resolver

### 9. Riscos / Bloqueios (se houver)

- **Risco 1**: descricao curta, impacto, owner de mitigacao
- **Risco 2**: ...

### 10. Proxima Reuniao / Follow-up

- Data/hora proposta (se houver)
- Objetivos para a proxima reuniao

### 11. Anexos / Referencias

- Documento de agenda: URL
- Slides: URL
- Transcricao / Gravacao: URL
- Tickets relacionados: lista de URLs ou IDs

### 12. Versao e Change Log

- **Versao**: 1.0
- **Ultima atualizacao**: YYYY-MM-DDTHH:MM:SSZ
