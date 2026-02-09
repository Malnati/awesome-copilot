# Contribuindo para o Awesome GitHub Copilot

Obrigado pelo seu interesse em contribuir para o repositorio Awesome GitHub Copilot! Damos boas-vindas as contribuicoes da comunidade para ajudar a expandir nossa colecao de instrucoes e prompts personalizados.

## Pre-requisitos

### Usuarios de Windows: habilitar symlinks

Este repositorio usa links simbolicos para plugins. No Windows, voce precisa habilitar suporte a symlinks antes de clonar:

1. **Habilitar Developer Mode** (recomendado):
   - Abra **Settings** -> **Update & Security** -> **For developers**
   - Habilite **Developer Mode**
   - Isso permite criar symlinks sem privilegios de administrador

2. **Configurar o Git para usar symlinks**:
   ```bash
   git config --global core.symlinks true
   ```

3. **Clonar o repositorio** (apos habilitar o acima):
   ```bash
   git clone https://github.com/Malnati/awesome-copilot.git
   ```

> **Nota:** Se voce clonou o repositorio antes de habilitar symlinks, eles aparecerao como arquivos de texto simples contendo o caminho de destino. Voce precisara excluir o repositorio local e clonar novamente apos habilitar o suporte a symlinks.

**Alternativa para versoes antigas do Windows:** se Developer Mode nao estiver disponivel, voce pode executar o Git Bash como Administrador ou conceder ao seu usuario o privilegio "Create symbolic links" via Local Security Policy (`secpol.msc` -> Local Policies -> User Rights Assignment -> Create symbolic links).

## Como Contribuir

### Adicionando Instructions

Instructions ajudam a personalizar o comportamento do GitHub Copilot para tecnologias, praticas de codificacao ou dominios especificos.

1. **Crie seu arquivo de instruction**: adicione um novo arquivo `.md` no diretorio `instructions/`
2. **Siga a convencao de nomes**: use nomes de arquivo descritivos, em minusculas e com hifens (por exemplo, `python-django.instructions.md`)
3. **Estruture seu conteudo**: comece com um titulo claro e organize suas instrucoes de forma logica
4. **Teste suas instructions**: garanta que suas instructions funcionem bem com o GitHub Copilot

#### Exemplo de formato de instruction

```markdown
---
description: 'Instructions for customizing GitHub Copilot behavior for specific technologies and practices'
---

# Your Technology/Framework Name

## Instructions

- Provide clear, specific guidance for GitHub Copilot
- Include best practices and conventions
- Use bullet points for easy reading

## Additional Guidelines

- Any additional context or examples
```

### Adicionando Prompts

Prompts sao templates prontos para uso para cenarios e tarefas especificas de desenvolvimento.

1. **Crie seu arquivo de prompt**: adicione um novo arquivo `.prompt.md` no diretorio `prompts/`
2. **Siga a convencao de nomes**: use nomes descritivos em minusculas com hifens e a extensao `.prompt.md` (por exemplo, `react-component-generator.prompt.md`)
3. **Inclua front matter**: adicione metadados no topo do arquivo (opcional, mas recomendado)
4. **Estruture seu prompt**: forneca contexto claro e instrucoes especificas

#### Exemplo de formato de prompt

```markdown
---
agent: 'agent'
tools: ['codebase', 'terminalCommand']
description: 'Brief description of what this prompt does'
---

# Prompt Title

Your goal is to...

## Specific Instructions

- Clear, actionable instructions
- Include examples where helpful
```

### Adicionando um Agent

Agents sao configuracoes especializadas que transformam o GitHub Copilot Chat em assistentes ou personas especificas de dominio para cenarios de desenvolvimento particulares.

1. **Crie seu arquivo de agent**: adicione um novo arquivo `.agent.md` no diretorio `agents/`
2. **Siga a convencao de nomes**: use nomes descritivos, em minusculas, com hifens e a extensao `.agent.md` (por exemplo, `react-performance-expert.agent.md`)
3. **Inclua front matter**: adicione metadados no topo do arquivo com campos obrigatorios
4. **Defina a persona**: crie uma identidade clara e area de especialidade para o agent
5. **Teste seu agent**: garanta que o agent forneca respostas uteis e precisas no dominio

#### Exemplo de formato de agent

```markdown
---
description: 'Brief description of the agent and its purpose'
model: 'gpt-5'
tools: ['codebase', 'terminalCommand']
name: 'My Agent Name'
---

You are an expert [domain/role] with deep knowledge in [specific areas].

## Your Expertise

- [Specific skill 1]
- [Specific skill 2]
- [Specific skill 3]

## Your Approach

- [How you help users]
- [Your communication style]
- [What you prioritize]

## Guidelines

- [Specific instructions for responses]
- [Constraints or limitations]
- [Best practices to follow]
```

### Adicionando Skills

Skills sao pastas autocontidas no diretorio `skills/` que incluem um arquivo `SKILL.md` (com front matter) e ativos empacotados opcionais.

1. **Crie uma nova pasta de skill**: execute `npm run skill:create -- --name <skill-name> --description "<skill description>"`
2. **Edite `SKILL.md`**: garanta que o `name` corresponda ao nome da pasta (minusculas com hifens) e que o `description` seja claro e nao vazio
3. **Adicione ativos opcionais**: mantenha os ativos empacotados com tamanho razoavel (menos de 5MB cada) e referencie-os no `SKILL.md`
4. **Valide e atualize docs**: execute `npm run skill:validate` e depois `npm run build` para atualizar as tabelas geradas no README

### Adicionando Collections

Collections agrupam prompts, instructions, agents e skills relacionados em torno de temas ou fluxos de trabalho especificos, facilitando a descoberta e adocao de toolkits completos.

1. **Crie seu manifesto de collection**: adicione um novo arquivo `.collection.yml` no diretorio `collections/`
2. **Siga a convencao de nomes**: use nomes descritivos, em minusculas, com hifens (por exemplo, `python-web-development.collection.yml`)
3. **Referencie itens existentes**: collections devem referenciar apenas arquivos que ja existem no repositorio
4. **Teste sua collection**: verifique se todos os arquivos referenciados existem e funcionam bem juntos

#### Criando uma collection

```bash
# Using the creation script
node create-collection.js my-collection-id

# Or using VS Code Task: Ctrl+Shift+P > "Tasks: Run Task" > "create-collection"
```

#### Exemplo de formato de collection

```yaml
id: my-collection-id
name: My Collection Name
description: A brief description of what this collection provides and who should use it.
tags: [tag1, tag2, tag3] # Optional discovery tags
items:
  - path: prompts/my-prompt.prompt.md
    kind: prompt
  - path: instructions/my-instructions.instructions.md
    kind: instruction
  - path: agents/my-custom.agent.md
    kind: agent
    usage: |
     recommended # or "optional" if not essential to the workflow

     This agent requires the following instructions/prompts/MCPs:
      - Instruction 1
      - Prompt 1
      - MCP 1

     This agent is ideal for...
      - Use case 1
      - Use case 2
    
      Here is an example of how to use it:
      ```markdown, task-plan.prompt.md
      ---
      mode: task-planner
      title: Plan microsoft fabric realtime intelligence terraform support
      ---
      #file: <file including in chat context>
      Do an action to achieve goal.
      ```

      To get the best results, consider...
      - Tip 1
      - Tip 2
    
display:
  ordering: alpha # or "manual" to preserve order above
  show_badge: false # set to true to show collection badge
```

Para exemplo completo de uso, confira a collection edge-ai tasks:
- [edge-ai-tasks.collection.yml](./collections/edge-ai-tasks.collection.yml)
- [edge-ai-tasks.md](./collections/edge-ai-tasks.md)

#### Diretrizes para Collections

- **Foque em fluxos de trabalho**: agrupe itens que funcionem juntos para casos de uso especificos
- **Tamanho razoavel**: normalmente 3-10 itens funcionam bem
- **Teste combinacoes**: garanta que os itens se complementem de forma eficaz
- **Objetivo claro**: a collection deve resolver um problema ou fluxo de trabalho especifico
- **Valide antes de enviar**: execute `node validate-collections.js` para garantir que seu manifesto e valido

### Trabalhando com Plugins

Plugins sao pacotes instalaveis gerados automaticamente a partir de collections. Eles contem agents, commands (prompts) e skills com symlinks do source da collection.

#### Criando um Plugin a partir de uma Collection

Quando voce cria uma nova collection, pode gerar o plugin correspondente:

```bash
# Migrate a collection to a new plugin (first time only)
npm run plugin:migrate -- --collection <collection-id>
```

#### Atualizando Plugins apos Mudancas na Collection

Se voce modificar uma collection (adicionar/remover itens, atualizar metadados), atualize o plugin correspondente:

```bash
# Refresh a single plugin
npm run plugin:refresh -- --collection <collection-id>

# Refresh all existing plugins
npm run plugin:refresh -- --all
```

#### Estrutura de Plugin

```plaintext
plugins/<collection-id>/
├── .github/plugin/plugin.json  # Metadados do plugin (auto-gerado)
├── README.md                   # Documentacao do plugin (auto-gerado)
├── agents/                     # Symlinks para arquivos de agent (.md)
├── commands/                   # Symlinks para arquivos de prompt (.md)
└── skills/                     # Symlinks para pastas de skill
```

#### Diretrizes de Plugin

- **Symlinks, nao copias**: arquivos do plugin sao symlinks para os arquivos de origem, evitando duplicacao
- **Instructions excluidas**: instructions nao sao suportadas atualmente em plugins
- **Conteudo auto-gerado**: `plugin.json` e `README.md` sao gerados a partir dos metadados da collection
- **Mantenha plugins sincronizados**: apos modificar uma collection, execute `plugin:refresh` para atualizar o plugin

## Enviando sua Contribuicao

1. **Fork este repositorio**
2. **Crie uma nova branch** para sua contribuicao
3. **Adicione sua instruction, prompt, chatmode ou collection** seguindo as diretrizes acima
4. **Execute o script de atualizacao**: `npm start` para atualizar o README com seu novo arquivo (certifique-se de executar `npm install` primeiro se ainda nao tiver feito)
   - Um workflow do GitHub Actions verificara se esse passo foi executado corretamente
   - Se o README.md seria modificado ao executar o script, o check do PR falhara com um comentario mostrando as alteracoes necessarias
5. **Envie um pull request** com:
   - Um titulo claro descrevendo sua contribuicao
   - Uma breve descricao do que sua instruction/prompt faz
   - Qualquer contexto relevante ou notas de uso

> [!NOTE]
> Usamos [all-contributors](https://github.com/all-contributors/all-contributors) para reconhecer todos os tipos de contribuicoes para o projeto. Va para [Contributors Recognition](#contributor-recognition) para saber mais!

## O que Aceitamos

Aceitamos contribuicoes que cubram qualquer tecnologia, framework ou pratica de desenvolvimento que ajude desenvolvedores a trabalhar de forma mais eficaz com o GitHub Copilot. Isso inclui:

- Linguagens de programacao e frameworks
- Metodologias de desenvolvimento e boas praticas
- Padroes de arquitetura e principios de design
- Estrategias de testes e garantia de qualidade
- Praticas de DevOps e implantacao
- Acessibilidade e design inclusivo
- Tecnicas de otimizacao de performance

## O que Nao Aceitamos

Para manter uma comunidade segura, responsavel e construtiva, **nao aceitaremos** contribuicoes que:

- **Violem os Responsible AI Principles**: Conteudo que tente contornar as diretrizes de Responsible AI da Microsoft/GitHub ou promova uso nocivo de AI
- **Comprometam a seguranca**: Instructions projetadas para burlar politicas de seguranca, explorar vulnerabilidades ou enfraquecer a seguranca do sistema
- **Habilitem atividades maliciosas**: Conteudo destinado a prejudicar outros sistemas, usuarios ou organizacoes
- **Explorem fragilidades**: Instructions que se aproveitam de vulnerabilidades em outras plataformas ou servicos
- **Promovam conteudo nocivo**: Orientacoes que possam levar a criacao de conteudo nocivo, discriminatorio ou inapropriado
- **Contornem politicas de plataforma**: Tentativas de contornar termos de servico do GitHub, Microsoft ou outras plataformas

## Diretrizes de Qualidade

- **Seja especifico**: Instructions genericas sao menos uteis do que orientacoes especificas e acionaveis
- **Teste seu conteudo**: Garanta que suas instructions ou prompts funcionem bem com o GitHub Copilot
- **Siga convencoes**: Use formatacao e nomenclatura consistentes
- **Mantenha foco**: Cada arquivo deve abordar uma tecnologia, framework ou caso de uso especifico
- **Escreva com clareza**: Use linguagem simples e direta
- **Promova boas praticas**: Incentive praticas de desenvolvimento seguras, sustentaveis e eticas

## Reconhecimento de Contribuidores

Usamos [all-contributors](https://github.com/all-contributors/all-contributors) para reconhecer **todos os tipos de contribuicoes** deste projeto.

Para se adicionar, deixe um comentario em uma issue ou pull request relevante usando seu username do GitHub e o(s) tipo(s) de contribuicao apropriado(s):

```markdown
@all-contributors add @username for contributionType1, contributionType2
```

A lista de contribuidores e atualizada automaticamente todo domingo as **3:00 AM UTC**. Quando a proxima execucao for concluida, seu nome aparecera na secao [README Contributors](./README.md#contributors-) .

### Tipos de Contribuicao

Aceitamos muitos tipos de contribuicao, incluindo as categorias personalizadas abaixo:

| Category | Description | Emoji |
| --- | --- | :---: |
| **Instructions** | Custom instruction sets that guide GitHub Copilot behavior | 🧭 |
| **Prompts** | Reusable or one-off prompts for GitHub Copilot | ⌨️ |
| **Agents** | Defined GitHub Copilot roles or personalities | 🎭 |
| **Skills** | Specialized knowledge of a task for GitHub Copilot | 🧰 |
| **Collections** | Curated bundles of related prompts, agents, or instructions | 🎁 |

Além disso, todos os tipos de contribuicao padrao suportados por [All Contributors](https://allcontributors.org/emoji-key/) sao reconhecidos.

> Toda contribuicao importa. Obrigado por ajudar a melhorar este recurso para a comunidade GitHub Copilot.


## Code of Conduct

Observe que este projeto e publicado com um [Contributor Code of Conduct](CODE_OF_CONDUCT.md). Ao participar deste projeto, voce concorda em seguir seus termos.

## License

Ao contribuir para este repositorio, voce concorda que suas contribuicoes serao licenciadas sob a MIT License.
