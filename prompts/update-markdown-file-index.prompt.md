---
agent: 'agent'
description: 'Atualize uma secao de arquivo markdown com um indice/tabela de arquivos de uma pasta especificada.'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'extensions', 'web/fetch', 'findTestFiles', 'githubRepo', 'openSimpleBrowser', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'search/searchResults', 'runCommands/terminalLastCommand', 'runCommands/terminalSelection', 'testFailure', 'usages', 'vscodeAPI']
---
# Atualizar Indice de Arquivo Markdown

Atualize o arquivo markdown `${file}` com um indice/tabela de arquivos da pasta `${input:folder}`.

## Processo

1. **Scan**: Leia o arquivo markdown alvo `${file}` para entender a estrutura existente
2. **Discover**: Liste todos os arquivos na pasta especificada `${input:folder}` que correspondam ao padrao `${input:pattern}`
3. **Analyze**: Identifique se existe uma secao de tabela/indice para atualizar ou criar uma nova estrutura
4. **Structure**: Gere o formato de tabela/lista apropriado com base nos tipos de arquivo e no conteudo existente
5. **Update**: Substitua a secao existente ou adicione nova secao com o indice de arquivos
6. **Validate**: Garanta que a sintaxe markdown seja valida e a formatacao consistente

## Analise de Arquivos

Para cada arquivo encontrado, extraia:

- **Name**: Nome do arquivo com ou sem extensao, conforme o contexto
- **Type**: Extensao e categoria (ex.: `.md`, `.js`, `.py`)
- **Description**: Primeiro comentario, cabecalho, ou proposito inferido
- **Size**: Tamanho do arquivo para referencia (opcional)
- **Modified**: Data da ultima modificacao (opcional)

## Opcoes de Estrutura de Tabela

Escolha o formato com base nos tipos de arquivo e no conteudo existente:

### Opcao 1: Lista Simples

```markdown
## Files in ${folder}

- [filename.ext](path/to/filename.ext) - Description
- [filename2.ext](path/to/filename2.ext) - Description
```

### Opcao 2: Tabela Detalhada

| File | Type | Description |
|------|------|-------------|
| [filename.ext](path/to/filename.ext) | Extension | Description |
| [filename2.ext](path/to/filename2.ext) | Extension | Description |

### Opcao 3: Secoes Categorizadas

Agrupe arquivos por tipo/categoria com secoes separadas ou sub-tabelas.

## Estrategia de Atualizacao

- 🔄 **Update existing**: Se a secao de tabela/indice existir, substitua o conteudo preservando a estrutura
- ➕ **Add new**: Se nao existir secao, crie uma nova com o formato mais adequado
- 📋 **Preserve**: Mantenha formatacao markdown existente, niveis de cabecalho e fluxo do documento
- 🔗 **Links**: Use caminhos relativos para links de arquivos dentro do repositorio

## Identificacao de Secao

Procure secoes existentes com estes padroes:

- Cabecalhos contendo: "index", "files", "contents", "directory", "list"
- Tabelas com colunas relacionadas a arquivos
- Listas com links de arquivos
- Comentarios HTML marcando secoes de indice de arquivos

## Requisitos

- Preservar estrutura e formatacao markdown existentes
- Usar caminhos relativos para links de arquivos
- Incluir descricoes de arquivos quando disponiveis
- Ordenar arquivos alfabeticamente por padrao
- Lidar com caracteres especiais em nomes de arquivos
- Validar toda a sintaxe markdown gerada
