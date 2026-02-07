---
name: make-skill-template
description: 'Crie novas Agent Skills para GitHub Copilot a partir de prompts ou duplicando este template. Use quando solicitado "criar uma skill", "fazer uma nova skill", "scaffold uma skill" ou ao construir capacidades de IA especializadas com recursos agrupados. Gera arquivos SKILL.md com frontmatter correto, estrutura de diretorio e pastas opcionais scripts/references/assets.'
---

# Template para Criar Skill

Uma meta-skill para criar novas Agent Skills. Use esta skill quando precisar fazer scaffold de uma nova pasta de skill, gerar um arquivo SKILL.md ou ajudar usuarios a entender a especificacao de Agent Skills.

## Quando Usar Esta Skill

- Usuario pede para "criar uma skill", "fazer uma nova skill" ou "scaffold uma skill"
- Usuario quer adicionar uma capacidade especializada ao setup do GitHub Copilot
- Usuario precisa de ajuda para estruturar uma skill com recursos agrupados
- Usuario quer duplicar este template como ponto de partida

## Pre-requisitos

- Entendimento do que a skill deve realizar
- Uma descricao clara, rica em keywords, de capacidades e gatilhos
- Conhecimento de recursos agrupados necessarios (scripts, references, assets, templates)

## Criando uma Nova Skill

### Etapa 1: Criar o Diretorio da Skill

Crie uma nova pasta com nome em minusculas e com hifens:

```
skills/<skill-name>/
└── SKILL.md          # Required
```

### Etapa 2: Gerar SKILL.md com Frontmatter

Cada skill requer frontmatter YAML com `name` e `description`:

```yaml
---
name: <skill-name>
description: '<What it does>. Use when <specific triggers, scenarios, keywords users might say>.'
---
```

#### Requisitos de Campos no Frontmatter

| Campo | Obrigatorio | Restricoes |
|-------|-------------|-----------|
| `name` | **Sim** | 1-64 chars, apenas letras/numeros/hifens minusculos, deve corresponder ao nome da pasta |
| `description` | **Sim** | 1-1024 chars, deve descrever O QUE faz E QUANDO usar |
| `license` | Nao | Nome de licenca ou referencia a LICENSE.txt incluido |
| `compatibility` | Nao | 1-500 chars, requisitos de ambiente se necessario |
| `metadata` | Nao | Pares chave-valor para propriedades adicionais |
| `allowed-tools` | Nao | Lista separada por espacos de tools pre-aprovadas (experimental) |

#### Boas Praticas para Descricao

**CRITICO**: A `description` e o PRINCIPAL mecanismo de descoberta automatica de skills. Inclua:

1. **O QUE** a skill faz (capacidades)
2. **QUANDO** usar (gatilhos, cenarios, tipos de arquivos)
3. **Keywords** que usuarios podem mencionar nos prompts

**Bom exemplo:**

```yaml
description: 'Toolkit for testing local web applications using Playwright. Use when asked to verify frontend functionality, debug UI behavior, capture browser screenshots, or view browser console logs. Supports Chrome, Firefox, and WebKit.'
```

**Exemplo ruim:**

```yaml
description: 'Web testing helpers'
```

### Etapa 3: Escrever o Corpo da Skill

Apos o frontmatter, adicione instrucoes em markdown. Secoes recomendadas:

| Secao | Proposito |
|---------|---------|
| `# Title` | Visao geral breve |
| `## When to Use This Skill` | Reforca gatilhos da descricao |
| `## Prerequisites` | Tools e dependencias requeridas |
| `## Step-by-Step Workflows` | Passos numerados para tarefas |
| `## Solucao de Problemas` | Issues comuns e solucoes |
| `## Referencias` | Links para docs incluidos |

### Etapa 4: Adicionar Diretorios Opcionais (se necessario)

| Pasta | Proposito | Quando usar |
|--------|---------|-------------|
| `scripts/` | Codigo executavel (Python, Bash, JS) | Automacao que executa operacoes |
| `references/` | Documentacao que o agente le | Referencias de API, schemas, guias |
| `assets/` | Arquivos estaticos usados AS-IS | Imagens, fontes, templates |
| `templates/` | Codigo inicial que o agente modifica | Scaffolds para estender |

## Exemplo: Estrutura Completa de Skill

```
my-awesome-skill/
├── SKILL.md                    # Required instructions
├── LICENSE.txt                 # Optional license file
├── scripts/
│   └── helper.py               # Executable automation
├── references/
│   ├── api-reference.md        # Detailed docs
│   └── examples.md             # Usage examples
├── assets/
│   └── diagram.png             # Static resources
└── templates/
    └── starter.ts              # Code scaffold
```

## Inicio Rapido: Duplicar Este Template

1. Copie a pasta `make-skill-template/`
2. Renomeie para o nome da sua skill (minusculas, hifens)
3. Atualize `SKILL.md`:
   - Troque `name:` para corresponder ao nome da pasta
   - Escreva uma `description:` rica em keywords
   - Substitua o conteudo do corpo pelas suas instrucoes
4. Adicione recursos agrupados conforme necessario
5. Valide com `npm run skill:validate`

## Checklist de Validacao

- [ ] Nome da pasta em minusculas com hifens
- [ ] Campo `name` corresponde exatamente ao nome da pasta
- [ ] `description` tem 10-1024 caracteres
- [ ] `description` explica O QUE e QUANDO
- [ ] `description` esta entre aspas simples
- [ ] Conteudo do corpo tem menos de 500 linhas
- [ ] Assets agrupados tem menos de 5MB cada

## Solucao de Problemas

| Problema | Solucao |
|---------|---------|
| Skill nao encontrada | Melhore a description com mais keywords e gatilhos |
| Validacao falha no nome | Garanta minusculas, sem hifens consecutivos, e que combina com a pasta |
| Description muito curta | Adicione capacidades, gatilhos e keywords |
| Assets nao encontrados | Use caminhos relativos a partir da raiz da skill |

## Referencias

- Agent Skills official spec: <https://agentskills.io/specification>