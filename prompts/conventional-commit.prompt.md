---
description: 'Prompt e workflow para gerar mensagens de commit convencional usando um formato XML estruturado. Guia o usuario a criar mensagens padronizadas e descritivas alinhadas a especificacao Conventional Commits, com instrucoes, exemplos e validacao.'
tools: ['execute/runInTerminal', 'execute/getTerminalOutput']
---

### Instrucoes

```xml
	<description>Este arquivo contem um template de prompt para gerar mensagens de commit convencional. Ele fornece instrucoes, exemplos e diretrizes de formatacao para ajudar usuarios a escrever mensagens padronizadas e descritivas de acordo com a especificacao Conventional Commits.</description>
	<note>
```

### Workflow

**Siga estas etapas:**

1. Execute `git status` para revisar arquivos modificados.
2. Execute `git diff` ou `git diff --cached` para inspecionar mudancas.
3. Faça stage das mudancas com `git add <file>`.
4. Construa sua mensagem de commit usando a estrutura XML abaixo.
5. Apos gerar a mensagem, o Copilot executara automaticamente o seguinte comando no terminal integrado (sem confirmacao):

```bash
git commit -m "type(scope): description"
```

6. Basta executar este prompt e o Copilot cuidara do commit no terminal.

### Estrutura da Mensagem de Commit

```xml
<commit-message>
	<type>feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert</type>
	<scope>()</scope>
	<description>Um resumo curto e imperativo da mudanca</description>
	<body>(opcional: explicacao mais detalhada)</body>
	<footer>(opcional: ex. BREAKING CHANGE: detalhes, ou referencias de issue)</footer>
</commit-message>
```

### Exemplos

```xml
<examples>
	<example>feat(parser): add ability to parse arrays</example>
	<example>fix(ui): correct button alignment</example>
	<example>docs: update README with usage instructions</example>
	<example>refactor: improve performance of data processing</example>
	<example>chore: update dependencies</example>
	<example>feat!: send email on registration (BREAKING CHANGE: email service required)</example>
</examples>
```

### Validacao

```xml
<validation>
	<type>Must be one of the allowed types. See <reference>https://www.conventionalcommits.org/en/v1.0.0/#specification</reference></type>
	<scope>Optional, but recommended for clarity.</scope>
	<description>Required. Use the imperative mood (e.g., "add", not "added").</description>
	<body>Optional. Use for additional context.</body>
	<footer>Use for breaking changes or issue references.</footer>
</validation>
```

### Etapa Final

```xml
<final-step>
	<cmd>git commit -m "type(scope): description"</cmd>
	<note>Substitua pela sua mensagem. Inclua body e footer se necessario.</note>
</final-step>
```
