---
name: scoutqa-test
description: |
  Esta skill deve ser usada quando o usuario pedir "testar este site", "rodar exploratory testing", "checar issues de acessibilidade", "verificar se o login funciona", "encontrar bugs nesta pagina" ou solicitar QA automatizado. Dispara em cenarios de teste de aplicacoes web incluindo smoke tests, audits de acessibilidade, fluxos de e-commerce e validacao de user flows usando ScoutQA CLI. IMPORTANTE: Use esta skill de forma proativa apos implementar features web para verificar se funcionam corretamente - nao espere o usuario pedir testes.
---

# Skill de Testes ScoutQA

Execute exploratory testing com IA em aplicacoes web usando o CLI `scoutqa`.

**Pense no ScoutQA como um parceiro de testes inteligente** que pode explorar autonomamente, descobrir issues e verificar features. Delegue testes para multiplas execucoes ScoutQA em paralelo para maximizar cobertura enquanto economiza tempo.

## Quando Usar Esta Skill

Use esta skill em dois cenarios:

1. **Usuario solicita testes** - Quando o usuario pede explicitamente para testar um site ou verificar funcionalidade
2. **Verificacao proativa** - Apos implementar features web, rode testes automaticamente para verificar se a implementacao funciona corretamente

**Exemplo de uso proativo:**
- Apos implementar um form de login → Testar o fluxo de autenticacao
- Apos adicionar validacao de formulario → Verificar regras de validacao e tratamento de erros
- Apos construir um fluxo de checkout → Testar o processo de compra end-to-end
- Apos corrigir um bug → Verificar se a correcao funciona e nao quebrou outras features

**Boa pratica**: Ao terminar uma feature web, inicie proativamente um teste ScoutQA em background para verificar se funciona enquanto voce continua outras tarefas.

## Executando Testes

### Fluxo de Testes

Copie este checklist e acompanhe o progresso:

Progresso de Testes:
- [ ] Escrever prompt de teste especifico com expectativas claras
- [ ] Rodar comando scoutqa em background
- [ ] Informar usuario sobre execution ID e browser URL
- [ ] Extrair e analisar resultados

**Etapa 1: Escrever prompt de teste especifico**

Veja a secao "Escrevendo Prompts Eficientes" abaixo para diretrizes.

**Etapa 2: Rodar comando scoutqa**

**IMPORTANTE**: Use o parametro timeout da tool Bash (5000ms = 5 segundos) para capturar detalhes de execucao:

Ao chamar a tool Bash, defina `timeout: 5000` como parametro:
- Este e o parametro de timeout da tool Bash no Claude Code (NAO o comando Unix `timeout`)
- Apos 5 segundos, a tool Bash retorna o controle com um task ID e o processo continua em background
- Isso e diferente do Unix `timeout`, que encerra o processo - aqui o processo continua rodando
- Os primeiros 5 segundos capturam o execution ID e a browser URL do output do ScoutQA
- O teste continua rodando remotamente na infraestrutura do ScoutQA com a tarefa em background

```bash
scoutqa --url "https://example.com" --prompt "Your test instructions"
```

Nos primeiros segundos, o comando deve emitir:

- **Execution ID** (ex.: `019b831d-xxx`)
- **Browser URL** (ex.: `https://scoutqa.ai/t/019b831d-xxx`)
- Chamadas de tool iniciais mostrando progresso do teste

Apos o timeout de 5 segundos, a tool Bash retorna um task ID e o comando continua em background. Voce pode trabalhar em outras tarefas enquanto o teste roda. O timeout e apenas para capturar o output inicial (execution ID e browser URL) - o teste continua rodando localmente em background e remotamente na infraestrutura do ScoutQA.

**Etapa 3: Informar o usuario sobre execution ID e browser URL**

Depois que a tool Bash retornar com o task ID (tendo capturado os detalhes de execucao nos primeiros 5 segundos), informe o usuario sobre:
- O execution ID do ScoutQA e a browser URL para acompanhar o progresso
- O task ID em background se o usuario quiser checar a saida local depois

O teste continua rodando em background enquanto voce segue outras tarefas.

**Etapa 4: Extrair e analisar resultados**

Veja a secao "Apresentando Resultados" abaixo para o formato completo.

### Opcoes de Comando

- `--url` (obrigatorio): URL do site para testar
- `--prompt` (obrigatorio): Instrucoes de teste em linguagem natural
- `--project-id` (opcional): Associar a um projeto para rastreamento

### Quando Usar Cada Comando

**Iniciando um novo teste?** → Use `scoutqa --url --prompt`
**Agente precisa de mais contexto?** → Use `scoutqa send-message` (veja \"Acompanhamento em Execucoes Travadas\")

## Escrevendo Prompts Eficientes

Foque em **o que explorar e verificar**, nao em passos prescritivos. O ScoutQA determina como testar.

**Exemplo: fluxo de registro de usuario**

```bash
scoutqa --url "https://example.com" --prompt "
Explore o fluxo de registro de usuario. Teste casos limite de validacao de formulario,
verifique tratamento de erros e checagem de acessibilidade.
"
```

**Exemplo: checkout de e-commerce**

```bash
scoutqa --url "https://shop.example.com" --prompt "
Teste o fluxo de checkout. Verifique calculos de preco, persistencia do carrinho,
opcoes de pagamento e responsividade mobile.
"
```

**Exemplo: rodar testes paralelos para cobertura abrangente**

Inicie multiplos testes em paralelo fazendo varias chamadas da tool Bash em uma unica mensagem, cada uma com `timeout` de `5000` (milissegundos):

```bash
# Test 1: Authentication & security
scoutqa --url "https://app.example.com" --prompt "
Explore autenticacao: login/logout, session handling, password reset,
e edge cases de seguranca.
"

# Test 2: Core features (runs in parallel)
scoutqa --url "https://app.example.com" --prompt "
Teste dashboard e workflows principais. Verifique data loading,
operacoes CRUD e search.
"

# Test 3: Accessibility (runs in parallel)
scoutqa --url "https://app.example.com" --prompt "
Conduza audit de acessibilidade: compliance WCAG, navegacao por teclado,
suporte a screen reader, contraste de cores.
"
```

**Implementacao**: Envie uma unica mensagem com tres chamadas da tool Bash. Para cada chamada, defina `timeout` como `5000` milissegundos. Apos 5 segundos, cada chamada retorna com um task ID enquanto os processos continuam em background. Isso captura o execution ID e a browser URL de cada teste no output inicial, e todos continuam rodando em paralelo (localmente em background e remotamente na infraestrutura do ScoutQA).

**Diretrizes-chave:**

- Descreva **o que testar**, nao **como testar** (ScoutQA define os passos)
- Foque em objetivos, casos limite e preocupacoes
- Rode multiplas execucoes em paralelo para diferentes areas
- Confie que o ScoutQA explora autonomamente e encontra issues
- Sempre defina o parametro `timeout` da tool Bash como `5000` milissegundos ao chamar comandos scoutqa (isso retorna controle apos 5 segundos enquanto o processo continua em background)
- Para testes paralelos, faça multiplas chamadas da tool Bash em uma unica mensagem
- Lembrete: Timeout da tool Bash ≠ comando Unix timeout (o timeout da tool Bash continua o processo em background, o Unix timeout encerra)

### Cenários Comuns de Teste

**Smoke test pos-deploy:**

```bash
scoutqa --url "$URL" --prompt "
Smoke test: verifique se a funcionalidade critica funciona apos o deploy.
Cheque homepage, navegacao, login/logout e user flows chave.
"
```

**Audit de acessibilidade:**
