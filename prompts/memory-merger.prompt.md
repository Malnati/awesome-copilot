---
description: 'Mescla aprendizados maduros de um arquivo de memoria de dominio no arquivo de instrucoes. Sintaxe: `/memory-merger >domain [scope]` onde scope e `global` (padrao), `user`, `workspace`, ou `ws`.'
---

# Memory Merger

Voce consolida aprendizados maduros do arquivo de memoria de um dominio no arquivo de instrucoes, garantindo preservacao do conhecimento com minima redundancia.

**Use a lista de tarefas** para acompanhar o progresso pelas etapas do processo e manter o usuario informado.

## Escopos

As instrucoes de memoria podem ser armazenadas em dois escopos:

- **Global** (`global` ou `user`) - Armazenado em `<global-prompts>` (`vscode-userdata:/User/prompts/`) e se aplica a todos os projetos VS Code
- **Workspace** (`workspace` ou `ws`) - Armazenado em `<workspace-instructions>` (`<workspace-root>/.github/instructions/`) e se aplica apenas ao projeto atual

O escopo padrao e **global**.

Ao longo deste prompt, `<global-prompts>` e `<workspace-instructions>` referem-se a esses diretorios.

## Sintaxe

```
/memory-merger >domain-name [scope]
```

- `>domain-name` - Obrigatorio. O dominio a mesclar (por exemplo, `>clojure`, `>git-workflow`, `>prompt-engineering`)
- `[scope]` - Opcional. Um de: `global`, `user` (ambos significam global), `workspace`, ou `ws`. Padrao: `global`

**Exemplos:**
- `/memory-merger >prompt-engineering` - mescla memorias globais de prompt engineering
- `/memory-merger >clojure workspace` - mescla memorias de clojure do workspace
- `/memory-merger >git-workflow ws` - mescla memorias de git-workflow do workspace

## Processo

### 1. Parse de Input e Leitura de Arquivos

- **Extraia** o dominio e o escopo do input do usuario
- **Determine** os caminhos de arquivo:
  - Global: `<global-prompts>/{domain}-memory.instructions.md` → `<global-prompts>/{domain}.instructions.md`
  - Workspace: `<workspace-instructions>/{domain}-memory.instructions.md` → `<workspace-instructions>/{domain}.instructions.md`
- O usuario pode ter digitado errado o dominio. Se nao encontrar o arquivo de memoria, faca glob no diretorio e determine se ha um possivel match. Pergunte ao usuario se houver duvida.
- **Leia** ambos os arquivos (o arquivo de memoria deve existir; o arquivo de instrucoes pode nao existir)

### 2. Analisar e Propor

Revise todas as secoes de memoria e apresente-as para consideracao de mesclagem:

```
## Proposed Memories for Merger

### Memory: [Headline]
**Content:** [Key points]
**Location:** [Where it fits in instructions]

[More memories]...
```

Diga: "Please review these memories. Approve all with 'go' or specify which to skip."

**PARE e aguarde o input do usuario.**

### 3. Definir a Quality Bar

Estabeleca criterios 10/10 para o que constitui um resultado de instrucoes mescladas excelente:
1. **Zero knowledge loss** - Cada detalhe, exemplo e nuance preservado
2. **Minimal redundancy** - Orientacao sobreposta consolidada
3. **Maximum scannability** - Hierarquia clara, estrutura paralela, negrito estrategico, agrupamento logico

### 4. Mesclar e Iterar

Desenvolva as instrucoes mescladas finais **sem atualizar arquivos ainda**:

1. Rascunhe as instrucoes mescladas incorporando memorias aprovadas
2. Avalie contra a quality bar
3. Refine estrutura, redacao e organizacao
4. Repita ate que as instrucoes atendam criterios 10/10

### 5. Atualizar Arquivos

Quando as instrucoes finais atenderem criterios 10/10:

- **Crie ou atualize** o arquivo de instrucoes com o conteudo final mesclado
  - Inclua frontmatter apropriado ao criar novo arquivo
  - **Mescle os patterns `applyTo`** dos arquivos de memoria e instrucao se ambos existirem, garantindo cobertura completa sem duplicacao
- **Remova** as secoes mescladas do arquivo de memoria

## Exemplo

```
User: "/memory-merger >clojure"

Agent:
1. Reads clojure-memory.instructions.md and clojure.instructions.md
2. Proposes 3 memories for merger
3. [STOPS]

User: "go"

Agent:
4. Defines quality bar for 10/10
5. Merges new instructions candidate, iterates to 10/10
6. Updates clojure.instructions.md
7. Cleans clojure-memory.instructions.md
```
