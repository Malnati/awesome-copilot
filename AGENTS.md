# AGENTS.md

## Visao Geral do Projeto

O repositorio Awesome GitHub Copilot e uma colecao conduzida pela comunidade de agentes personalizados, prompts e instrucoes criada para aprimorar experiencias do GitHub Copilot em diferentes dominios, linguagens e casos de uso. O projeto inclui:

- **Agents** - Agentes especializados do GitHub Copilot que integram com servidores MCP
- **Prompts** - Prompts especificos para tarefas de geracao de codigo e resolucao de problemas
- **Instructions** - Padroes de codificacao e boas praticas aplicadas a padroes especificos de arquivos
- **Skills** - Pastas autocontidas com instrucoes e recursos empacotados para tarefas especializadas
- **Collections** - Colecoes curadas organizadas em torno de temas e fluxos de trabalho especificos

## Estrutura do Repositorio

```
.
├── agents/           # Definicoes de agentes personalizados do GitHub Copilot (arquivos .agent.md)
├── prompts/          # Prompts especificos para tarefas (arquivos .prompt.md)
├── instructions/     # Padroes e diretrizes de codificacao (arquivos .instructions.md)
├── skills/           # Pastas de Agent Skills (cada uma com SKILL.md e ativos opcionais)
├── collections/      # Colecoes curadas de recursos (arquivos .md)
├── docs/             # Documentacao para diferentes tipos de recursos
├── eng/              # Scripts de build e automacao
└── scripts/          # Scripts utilitarios
```

## Comandos de Setup

```bash
# Install dependencies
npm ci

# Build the project (generates README.md)
npm run build

# Validate collection manifests
npm run collection:validate

# Create a new collection
npm run collection:create -- --id <collection-id> --tags <tags>

# Validate agent skills
npm run skill:validate

# Create a new skill
npm run skill:create -- --name <skill-name>
```

## Fluxo de Desenvolvimento

### Trabalhando com Agents, Prompts, Instructions e Skills

Todos os arquivos de agentes (`*.agent.md`), prompts (`*.prompt.md`) e instrucoes (`*.instructions.md`) devem incluir front matter markdown apropriado. Agent Skills sao pastas que contem um arquivo `SKILL.md` com front matter e ativos empacotados opcionais:

#### Arquivos de Agents (*.agent.md)
- Devem ter o campo `description` (envolto em aspas simples)
- Os nomes dos arquivos devem ser em letras minusculas com palavras separadas por hifens
- Recomendado incluir o campo `tools`
- Fortemente recomendado especificar o campo `model`

#### Arquivos de Prompts (*.prompt.md)
- Devem ter o campo `agent` (o valor deve ser `'agent'` envolto em aspas simples)
- Devem ter o campo `description` (envolto em aspas simples, nao vazio)
- Os nomes dos arquivos devem ser em letras minusculas com palavras separadas por hifens
- Recomendado especificar `tools` quando aplicavel
- Fortemente recomendado especificar o campo `model`

#### Arquivos de Instructions (*.instructions.md)
- Devem ter o campo `description` (envolto em aspas simples, nao vazio)
- Devem ter o campo `applyTo` especificando padroes de arquivos (por exemplo, `'**.js, **.ts'`)
- Os nomes dos arquivos devem ser em letras minusculas com palavras separadas por hifens

#### Agent Skills (skills/*/SKILL.md)
- Cada skill e uma pasta que contem um arquivo `SKILL.md`
- O `SKILL.md` deve ter o campo `name` (minusculo com hifens, correspondendo ao nome da pasta, max 64 caracteres)
- O `SKILL.md` deve ter o campo `description` (envolto em aspas simples, 10-1024 caracteres)
- Os nomes das pastas devem ser em letras minusculas com palavras separadas por hifens
- Skills podem incluir ativos empacotados (scripts, templates, arquivos de dados)
- Ativos empacotados devem ser referenciados nas instrucoes do `SKILL.md`
- Arquivos de ativos devem ter tamanho razoavel (menos de 5MB por arquivo)
- Skills seguem a [Agent Skills specification](https://agentskills.io/specification)

### Adicionando Novos Recursos

Ao adicionar um novo agent, prompt, instruction ou skill:

**Para Agents, Prompts e Instructions:**
1. Crie o arquivo com o front matter apropriado
2. Adicione o arquivo ao diretorio apropriado
3. Atualize o README.md executando: `npm run build`
4. Verifique se o recurso aparece no README gerado

**Para Skills:**
1. Execute `npm run skill:create` para criar o esqueleto de uma nova pasta de skill
2. Edite o arquivo `SKILL.md` gerado com suas instrucoes
3. Adicione quaisquer ativos empacotados (scripts, templates, dados) a pasta de skill
4. Execute `npm run skill:validate` para validar a estrutura da skill
5. Atualize o README.md executando: `npm run build`
6. Verifique se a skill aparece no README

### Instrucoes de Teste

```bash
# Run all validation checks
npm run collection:validate
npm run skill:validate

# Build and verify README generation
npm run build

# Fix line endings (required before committing)
bash scripts/fix-line-endings.sh
```

Antes de commitar:
- Garanta que todo front matter markdown esteja corretamente formatado
- Verifique se os nomes de arquivo seguem a convencao de minusculas com hifens
- Execute `npm run build` para atualizar o README
- **Sempre execute `bash scripts/fix-line-endings.sh`** para normalizar as quebras de linha (CRLF -> LF)
- Verifique se seu novo recurso aparece corretamente no README

## Diretrizes de Estilo de Codigo

### Arquivos Markdown
- Use front matter apropriado com os campos obrigatorios
- Mantenha descricoes concisas e informativas
- Envolva valores do campo description em aspas simples
- Use nomes de arquivo em letras minusculas com hifens como separadores

### Scripts JavaScript/Node.js
- Localizados nos diretorios `eng/` e `scripts/`
- Sigam convencoes de ES module do Node.js (extensao `.mjs`)
- Use nomes de funcoes e variaveis claros e descritivos

## Diretrizes para Pull Request

Ao criar um pull request:

1. **Atualizacoes do README**: Novos arquivos devem ser adicionados automaticamente ao README quando voce executar `npm run build`
2. **Validacao de front matter**: Garanta que todos os arquivos markdown tenham os campos obrigatorios no front matter
3. **Nomenclatura de arquivos**: Verifique se todos os novos arquivos seguem a convencao de nomes em minusculas com hifens
4. **Verificacao de build**: Execute `npm run build` antes de commitar para verificar a geracao do README
5. **Quebras de linha**: **Sempre execute `bash scripts/fix-line-endings.sh`** para normalizar as quebras de linha para LF (estilo Unix)
6. **Descricao**: Forneca uma descricao clara do que seu agent/prompt/instruction faz
7. **Testes**: Se estiver adicionando uma collection, execute `npm run collection:validate` para garantir validade

### Checklist Pre-commit

Antes de enviar seu PR, garanta que voce:
- [ ] Run `npm install` (ou `npm ci`) para instalar as dependencias
- [ ] Run `npm run build` para gerar o README.md atualizado
- [ ] Run `bash scripts/fix-line-endings.sh` para normalizar as quebras de linha
- [ ] Verificou que todos os novos arquivos tem front matter apropriado
- [ ] Testou que sua contribuicao funciona com o GitHub Copilot
- [ ] Conferiu que os nomes dos arquivos seguem a convencao

### Checklist de Code Review

Para arquivos de prompt (*.prompt.md):
- [ ] Tem front matter markdown
- [ ] Tem campo `agent` (o valor deve ser `'agent'` envolto em aspas simples)
- [ ] Tem campo `description` nao vazio envolto em aspas simples
- [ ] Nome do arquivo em minusculas com hifens
- [ ] Inclui campo `model` (fortemente recomendado)

Para arquivos de instruction (*.instructions.md):
- [ ] Tem front matter markdown
- [ ] Tem campo `description` nao vazio envolto em aspas simples
- [ ] Tem campo `applyTo` com padroes de arquivos
- [ ] Nome do arquivo em minusculas com hifens

Para arquivos de agent (*.agent.md):
- [ ] Tem front matter markdown
- [ ] Tem campo `description` nao vazio envolto em aspas simples
- [ ] Tem campo `name` com nome legivel por humanos (por exemplo, "Address Comments" e nao "address-comments")
- [ ] Nome do arquivo em minusculas com hifens
- [ ] Inclui campo `model` (fortemente recomendado)
- [ ] Considera usar o campo `tools`

Para skills (skills/*/):
- [ ] A pasta contem um arquivo `SKILL.md`
- [ ] O `SKILL.md` tem front matter markdown
- [ ] Tem campo `name` correspondendo ao nome da pasta (minusculas com hifens, max 64 caracteres)
- [ ] Tem campo `description` nao vazio envolto em aspas simples (10-1024 caracteres)
- [ ] Nome da pasta em minusculas com hifens
- [ ] Quaisquer ativos empacotados sao referenciados no `SKILL.md`
- [ ] Os ativos empacotados tem menos de 5MB por arquivo

## Contribuicao

Este e um projeto conduzido pela comunidade. Contribuicoes sao bem-vindas! Consulte:
- [CONTRIBUTING.md](CONTRIBUTING.md) para diretrizes de contribuicao
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) para padroes da comunidade
- [SECURITY.md](SECURITY.md) para politicas de seguranca

## MCP Server

O repositorio inclui um servidor MCP (Model Context Protocol) que fornece prompts para buscar e instalar recursos diretamente deste repositorio. Docker e necessario para executar o servidor.

## Licenca

MIT License - veja [LICENSE](LICENSE) para detalhes.
