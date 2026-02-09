---
description: 'Criador inteligente de branches Git Flow que analisa git status/diff e cria branches apropriadas seguindo o modelo nvie Git Flow.'
tools: ['runCommands/runInTerminal', 'runCommands/getTerminalOutput']
agent: 'agent'
---

### Instrucoes

```xml
<instructions>
	<title>Git Flow Branch Creator</title>
	<description>Este prompt analisa suas mudancas atuais no git usando git status e git diff (ou git diff --cached), depois determina inteligentemente o tipo de branch apropriado conforme o modelo Git Flow e cria um nome semantico.</description>
	<note>
		Basta executar este prompt e o Copilot vai analisar as mudancas e criar a branch Git Flow apropriada.
	</note>
</instructions>
```

### Workflow

**Siga estas etapas:**

1. Execute `git status` para revisar o estado atual do repositorio e arquivos modificados.
2. Execute `git diff` (para mudancas nao staged) ou `git diff --cached` (para staged) para analisar a natureza das mudancas.
3. Analise as mudancas usando o Git Flow Branch Analysis Framework abaixo.
4. Determine o tipo de branch apropriado com base na analise.
5. Gere um nome semantico de branch seguindo convencoes do Git Flow.
6. Crie a branch e alterne para ela automaticamente.
7. Forneca um resumo da analise e proximos passos.

### Git Flow Branch Analysis Framework

```xml
<analysis-framework>
	<branch-types>
		<feature>
			<purpose>Novas features, melhorias, evolucoes nao criticas</purpose>
			<branch-from>develop</branch-from>
			<merge-to>develop</merge-to>
			<naming>feature/descriptive-name ou feature/ticket-number-description</naming>
			<indicators>
				<indicator>Nova funcionalidade sendo adicionada</indicator>
				<indicator>Melhorias de UI/UX</indicator>
				<indicator>Novos endpoints ou metodos de API</indicator>
				<indicator>Adicoes de schema de banco (nao breaking)</indicator>
				<indicator>Novas opcoes de configuracao</indicator>
				<indicator>Melhorias de performance (nao criticas)</indicator>
			</indicators>
		</feature>

		<release>
			<purpose>Preparacao de release, bump de versao, testes finais</purpose>
			<branch-from>develop</branch-from>
			<merge-to>develop AND master</merge-to>
			<naming>release-X.Y.Z</naming>
			<indicators>
				<indicator>Mudancas de versao</indicator>
				<indicator>Atualizacoes de configuracao de build</indicator>
				<indicator>Finalizacao de documentacao</indicator>
				<indicator>Bug fixes menores antes do release</indicator>
				<indicator>Atualizacoes de release notes</indicator>
				<indicator>Locks de versao de dependencias</indicator>
			</indicators>
		</release>

		<hotfix>
			<purpose>Correcoes criticas de producao que exigem deploy imediato</purpose>
			<branch-from>master</branch-from>
			<merge-to>develop AND master</merge-to>
			<naming>hotfix-X.Y.Z ou hotfix/critical-issue-description</naming>
			<indicators>
				<indicator>Fix de vulnerabilidade de seguranca</indicator>
				<indicator>Bug critico em producao</indicator>
				<indicator>Fix de corrupcao de dados</indicator>
				<indicator>Resolucao de outage</indicator>
				<indicator>Mudancas emergenciais de configuracao</indicator>
			</indicators>
		</hotfix>
	</branch-types>
</analysis-framework>
```

### Convencoes de Nome de Branch

```xml
<naming-conventions>
	<feature-branches>
		<format>feature/[ticket-number-]descriptive-name</format>
		<examples>
			<example>feature/user-authentication</example>
			<example>feature/PROJ-123-shopping-cart</example>
			<example>feature/api-rate-limiting</example>
			<example>feature/dashboard-redesign</example>
		</examples>
	</feature-branches>

	<release-branches>
		<format>release-X.Y.Z</format>
		<examples>
			<example>release-1.2.0</example>
			<example>release-2.1.0</example>
			<example>release-1.0.0</example>
		</examples>
	</release-branches>

	<hotfix-branches>
		<format>hotfix-X.Y.Z OR hotfix/critical-description</format>
		<examples>
			<example>hotfix-1.2.1</example>
			<example>hotfix/security-patch</example>
			<example>hotfix/payment-gateway-fix</example>
			<example>hotfix-2.1.1</example>
		</examples>
	</hotfix-branches>
</naming-conventions>
```

### Analysis Process

```xml
<analysis-process>
	<step-1>
		<title>Change Nature Analysis</title>
		<description>Examine os tipos de arquivos modificados e a natureza das mudancas</description>
		<criteria>
			<files-modified>Observe extensoes, estrutura de diretorios e proposito</files-modified>
			<change-scope>Determine se as mudancas sao aditivas, corretivas ou preparatorias</change-scope>
			<urgency-level>Avalie se as mudancas tratam issues criticas ou sao evolutivas</urgency-level>
		</criteria>
	</step-1>

	<step-2>
		<title>Git Flow Classification</title>
		<description>Mapeie as mudancas para o tipo de branch apropriado</description>
		<decision-tree>
			<question>Estas mudancas sao correcoes criticas para producao?</question>
			<if-yes>Considere branch hotfix</if-yes>
			<if-no>
				<question>Estas mudancas sao preparacao de release (bump de versao, ajustes finais)?</question>
				<if-yes>Considere branch release</if-yes>
				<if-no>Default para feature branch</if-no>
			</if-no>
		</decision-tree>
	</step-2>

	<step-3>
		<title>Branch Name Generation</title>
		<description>Crie nome semantico e descritivo</description>
		<guidelines>
			<use-kebab-case>Use lowercase com hifens</use-kebab-case>
			<be-descriptive>O nome deve indicar claramente o proposito</be-descriptive>
			<include-context>Inclua ticket ou contexto quando disponivel</include-context>
			<keep-concise>Evite nomes longos demais</keep-concise>
		</guidelines>
	</step-3>
</analysis-process>
```

### Edge Cases and Validation

```xml
<edge-cases>
	<mixed-changes>
		<scenario>Mudancas incluem features e bug fixes</scenario>
		<resolution>Priorize o tipo mais significativo ou sugira separar em multiplas branches</resolution>
	</mixed-changes>

	<no-changes>
		<scenario>Nenhuma mudanca detectada em git status/diff</scenario>
		<resolution>Informe o usuario e sugira checar git status ou fazer mudancas</resolution>
	</no-changes>

	<existing-branch>
		<scenario>Ja esta em uma branch feature/hotfix/release</scenario>
		<resolution>Analise se nova branch e necessaria ou se a atual e apropriada</resolution>
	</existing-branch>

	<conflicting-names>
		<scenario>Nome sugerido ja existe</scenario>
		<resolution>Adicione sufixo incremental ou sugira nome alternativo</resolution>
	</conflicting-names>
</edge-cases>
```

### Exemplos

```xml
<examples>
	<example-1>
		<scenario>Added new user registration API endpoint</scenario>
		<analysis>Nova funcionalidade, mudancas aditivas, nao criticas</analysis>
		<branch-type>feature</branch-type>
		<branch-name>feature/user-registration-api</branch-name>
		<command>git checkout -b feature/user-registration-api develop</command>
	</example-1>

	<example-2>
		<scenario>Fixed critical security vulnerability in authentication</scenario>
		<analysis>Fix de seguranca, critico para producao, deploy imediato</analysis>
		<branch-type>hotfix</branch-type>
		<branch-name>hotfix/auth-security-patch</branch-name>
		<command>git checkout -b hotfix/auth-security-patch master</command>
	</example-2>

	<example-3>
		<scenario>Updated version to 2.1.0 and finalized release notes</scenario>
		<analysis>Preparacao de release, bump de versao, documentacao</analysis>
		<branch-type>release</branch-type>
		<branch-name>release-2.1.0</branch-name>
		<command>git checkout -b release-2.1.0 develop</command>
	</example-3>

	<example-4>
		<scenario>Improved database query performance and updated caching</scenario>
		<analysis>Melhoria de performance, evolucao nao critica</analysis>
		<branch-type>feature</branch-type>
		<branch-name>feature/database-performance-optimization</branch-name>
		<command>git checkout -b feature/database-performance-optimization develop</command>
	</example-4>
</examples>
```

### Validation Checklist

```xml
<validation>
	<pre-analysis>
		<check>Repositorio esta limpo (sem changes nao commitadas que conflitem)</check>
		<check>Branch atual e ponto de partida adequado (develop para features/releases, master para hotfixes)</check>
		<check>Repositorio remoto esta atualizado</check>
	</pre-analysis>

	<analysis-quality>
		<check>Analise de mudancas cobre todos os arquivos modificados</check>
		<check>Selecao do tipo de branch segue Git Flow</check>
		<check>Nome de branch e semantico e segue convencoes</check>
		<check>Edge cases considerados e tratados</check>
	</analysis-quality>

	<execution-safety>
		<check>Branch alvo (develop/master) existe e esta acessivel</check>
		<check>Nome proposto nao conflita com branches existentes</check>
		<check>Usuario tem permissoes para criar branches</check>
	</execution-safety>
</validation>
```

### Final Execution

```xml
<execution-protocol>
	<analysis-summary>
		<git-status>Output of git status command</git-status>
		<git-diff>Relevant portions of git diff output</git-diff>
		<change-analysis>Detailed analysis of what changes represent</change-analysis>
		<branch-decision>Explanation of why specific branch type was chosen</branch-decision>
	</analysis-summary>

	<branch-creation>
		<command>git checkout -b [branch-name] [source-branch]</command>
		<confirmation>Verify branch creation and current branch status</confirmation>
		<next-steps>Provide guidance on next actions (commit changes, push branch, etc.)</next-steps>
	</branch-creation>

	<fallback-options>
		<alternative-names>Suggest 2-3 alternative branch names if primary suggestion isn't suitable</alternative-names>
		<manual-override>Allow user to specify different branch type if analysis seems incorrect</manual-override>
	</fallback-options>
</execution-protocol>
```

### Git Flow Reference

```xml
<gitflow-reference>
	<main-branches>
		<master>Codigo pronto para producao, cada commit e um release</master>
		<develop>Branch de integracao para features, ultimas mudancas</develop>
	</main-branches>

	<supporting-branches>
		<feature>Branch a partir de develop, merge de volta em develop</feature>
		<release>Branch a partir de develop, merge para develop e master</release>
		<hotfix>Branch a partir de master, merge para develop e master</hotfix>
	</supporting-branches>

	<merge-strategy>
		<flag>Use sempre --no-ff para preservar historico</flag>
		<tagging>Tagueie releases na branch master</tagging>
		<cleanup>Delete branches apos merge bem-sucedido</cleanup>
	</merge-strategy>
</gitflow-reference>
```
