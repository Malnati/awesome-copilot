---
name: workiq-copilot
description: 'Orienta o Copilot CLI sobre como usar o WorkIQ CLI/MCP server para consultar dados do Microsoft 365 Copilot (emails, meetings, docs, Teams, people) com contexto ao vivo, resumos e recomendacoes.'
---

# Skill do WorkIQ Copilot

## Visao Geral

WorkIQ (Public Preview) permite que o Copilot consulte dados do Microsoft 365 com linguagem natural. Ele suporta agendas, documentos, mensagens do Teams, threads de email, acompanhamento de follow-up, resumos de stakeholders e mais. Use esta skill sempre que a tarefa exigir inteligencia organizacional ao vivo alem do repositorio local.

## Dados Suportados e Prompts de Exemplo

- **Emails** – “Resuma emails da Sarah sobre o budget.”
- **Meetings** – “Quais sao minhas meetings desta semana?”
- **Documents** – “Encontre documents recentes sobre Q4 planning.”
- **Teams** – “Resuma mensagens no canal Engineering hoje.”
- **People/Projects** – “Quem esta trabalhando no Project Alpha?”

## Como Obter Acesso

1. **Copilot CLI plugin (preferido)**
   - `copilot`
   - `/plugin marketplace add github/copilot-plugins`
   - `/plugin install workiq@copilot-plugins`
   - Reinicie o Copilot CLI.
2. **CLI standalone / MCP server**
   - `npm install -g @microsoft/workiq` (ou `npx -y @microsoft/workiq mcp`).
   - Execute `workiq mcp` para expor ferramentas MCP se necessario.
3. **Consentimento do tenant**
   - O primeiro uso solicita consentimento do admin do Microsoft 365 (EULA + permissoes). Nao-admins devem contatar o admin do tenant para aprovar conforme o Tenant Administrator Enablement Guide.

## Checklist de Pre-flight

- Execute `Get-Command workiq` para confirmar que o binario esta disponivel.
- Aceite a EULA uma vez com `workiq accept-eula`.
- Confirme o tenant correto (`-t <tenant-id>` se diferente do default `common`).
- Esteja pronto para concluir o login por dispositivo no browser quando solicitado.

## Fluxo Principal

1. **Esclarecer intencao** – agenda, action items, busca de documentos, people search, resumo de riscos, etc.
2. **Criar prompt preciso** – inclua periodo, fonte ou topico (ex.: “Resuma posts do Teams em #eng de hoje”).
3. **Executar comando** – `workiq ask --question "<prompt>"` (use `-q` como atalho se desejar).
4. **Monitorar execucao** – respostas longas podem ser exibidas em streaming; aguarde finalizar antes de novos pedidos.
5. **Resumir e redigir** – destaque insights, conflitos/tarefas, evite colar links brutos a menos que seja necessario.
6. **Oferecer follow-ups** – bloquear tempo, redigir notas, consultas mais profundas, etc.

## Referencia de Comandos

| Comando                           | Proposito                                                       |
| --------------------------------- | --------------------------------------------------------------- |
| `workiq --help`                   | Mostrar opcoes globais.                                         |
| `workiq version`                  | Exibir versao instalada.                                        |
| `workiq accept-eula`              | Aceitar licenca (primeiro uso).                                 |
| `workiq ask`                      | Modo interativo.                                                |
| `workiq ask --question "..."`     | Fazer uma pergunta especifica (use `-q` se preferir).           |
| `workiq ask -t <tenant> -q "..."` | Mirar um tenant especifico.                                     |
| `workiq mcp`                      | Iniciar servidor MCP stdio (expor ferramentas WorkIQ).          |

## Padroes de Prompt

- Agenda: “O que ha no meu calendar amanha?”
- Action items: “Resuma follow-ups do customer sync de hoje.”
- Documentos: “Liste PowerPoints sobre o roadmap FY26 da Contoso.”
- Communications: “O que meu manager disse sobre o deadline?”
- Insights: “Quais blockers surgiram nas ultimas tres meetings?”
- Planning: “Sugira focus blocks para a tarde de Tuesday.”

## Diretrizes de Resposta

- Mantenha resumos concisos (2–3 frases), destacando carga, prioridades, bloqueios e proximos passos opcionais.
- Cite meetings/documents de forma generica, a menos que o usuario precise especificamente de links.
- Mencione se o WorkIQ pode continuar (ex.: “WorkIQ pode mostrar Thu–Sun se necessario”).
- Converta acoes sugeridas pelo WorkIQ em ofertas claras (bloquear tempo, enviar follow-up, solicitar gravacao, fazer consulta mais profunda).

## Boas Praticas

- Prefira prompts focados para reduzir ruido; execute multiplas consultas se necessario.
- Combine outputs de forma logica (agenda + conflitos + action items) antes de responder.
- Respeite privacidade: nao exponha listas de participantes ou trechos confidenciais sem solicitacao explicita.
- Registre quais comandos foram executados para que proximos passos possam referenciar (“Asked WorkIQ for agenda + conflicts”).
- Use modo MCP (`workiq mcp`) quando outro agent/workflow precisar de acesso direto a ferramentas.

## Solucao de Problemas

- **CLI ausente (Missing CLI)** – instale via npm ou verifique o PATH; notifique o usuario se indisponivel.
- **Erros de consent/auth** – reexecute apos o admin conceder permissoes ou apos concluir login por dispositivo.
- **Output longo/incompleto** – refaça com escopo mais preciso ou solicite fatias especificas (por dia/projeto/pessoa).
- **Comando travado** – cancele o comando no terminal (ex.: Ctrl+C) ou reinicie a sessao do Copilot CLI e tente novamente; garanta que o login no browser foi concluido.

## Acoes de Follow-up a Oferecer

- Bloquear focus/overflow holds em horarios sugeridos.
- Rascunhar mensagens de reagendamento/recusa com base na orientacao do WorkIQ.
- Solicitar gravacoes ou resumos para sessoes sobrepostas.
- Registrar action items em rastreadores de tarefas.
- Executar consultas adicionais no WorkIQ (por projeto, stakeholder, intervalo de tempo) para analise mais profunda.
