# Template de Colecoes

Use este template para criar uma nova colecao de prompts, instructions e chat modes relacionados.

## Template Basico

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
  - path: agents/my-chatmode.agent.md
    kind: agent
display:
  ordering: alpha # or "manual" to preserve order above
  show_badge: false # set to true to show collection badge
```

## Descricoes dos Campos

- **id**: Identificador unico usando apenas letras minusculas, numeros e hifens
- **name**: Nome exibido da colecao
- **description**: Breve explicacao do proposito da colecao (1-500 caracteres)
- **tags**: Array opcional de tags de descoberta (max 10, cada uma com 1-30 caracteres)
- **items**: Array de itens na colecao (1-50 itens)
- **path**: Caminho relativo da raiz do repositorio ate o arquivo
- **kind**: Deve ser `prompt`, `instruction` ou `chat-mode`
- **display**: Configuracoes opcionais de exibicao
- **ordering**: `alpha` (alfabetica) ou `manual` (preserva a ordem)
- **show_badge**: Exibir badge da colecao nos itens (true/false)

## Criando uma Nova Colecao

### Usando Tarefas do VS Code
1. Pressione `Ctrl+Shift+P` (ou `Cmd+Shift+P` no Mac)
2. Digite "Tasks: Run Task"
3. Selecione "create-collection"
4. Informe o ID da colecao quando solicitado

### Usando Linha de Comando
```bash
node create-collection.js my-collection-id
```

### Criacao Manual
1. Crie `collections/my-collection-id.collection.yml`
2. Use o template acima como ponto de partida
3. Adicione seus itens e personalize as configuracoes
4. Execute `npm run validate:collections` para validar
5. Execute `npm start` para gerar a documentacao

## Validacao

As colecoes sao validadas automaticamente para garantir:
- Campos obrigatorios presentes e validos
- Caminhos de arquivos existentes e que correspondem ao tipo de item
- IDs unicos entre as colecoes
- Tags e configuracoes de exibicao seguem o schema

Para validar manualmente:
```bash
npm run validate:collections
```

## Organizacao de Arquivos

As colecoes nao exigem reorganizar arquivos existentes. Itens podem ficar em qualquer lugar no repositorio desde que os caminhos estejam corretos no manifesto.

## Melhores Praticas

1. **Colecoes Significativas**: Agrupe itens que funcionam bem juntos para um fluxo de trabalho ou caso de uso especifico
2. **Nomeacao Clara**: Use nomes e IDs descritivos que reflitam o proposito da colecao
3. **Boas Descricoes**: Explique quem deve usar a colecao e qual beneficio ela oferece
4. **Tags Relevantes**: Adicione tags de descoberta que ajudem os usuarios a encontrar colecoes relacionadas
5. **Tamanho Razoavel**: Mantenha as colecoes focadas - normalmente 3-10 itens funcionam bem
6. **Testar Itens**: Garanta que todos os arquivos referenciados existam e funcionem antes de adicionar a uma colecao
