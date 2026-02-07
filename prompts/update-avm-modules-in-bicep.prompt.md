---
agent: 'agent'
description: 'Atualize Azure Verified Modules (AVM) para as versoes mais recentes em arquivos Bicep.'
tools: ['search/codebase', 'think', 'changes', 'web/fetch', 'search/searchResults', 'todos', 'edit/editFiles', 'search', 'runCommands', 'bicepschema', 'azure_get_schema_for_Bicep']
---
# Atualizar Azure Verified Modules em Arquivos Bicep

Atualize o arquivo Bicep `${file}` para usar as versoes mais recentes do Azure Verified Module (AVM). Limite atualizacoes de progresso a mudancas sem quebra. Nao apresente informacoes alem da tabela final de saida e do resumo.

## Processo

1. **Scan**: Extraia modulos AVM e versoes atuais de `${file}`
1. **Identify**: Liste todos os modulos AVM unicos usados ao corresponder `avm/res/{service}/{resource}` usando a ferramenta `#search`
1. **Check**: Use a ferramenta `#fetch` para obter a versao mais recente de cada modulo AVM no MCR: `https://mcr.microsoft.com/v2/bicep/avm/res/{service}/{resource}/tags/list`
1. **Compare**: Analise versoes semanticas para identificar modulos AVM que precisam de atualizacao
1. **Review**: Para mudancas com quebra, use a ferramenta `#fetch` para obter docs de: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/res/{service}/{resource}`
1. **Update**: Aplique atualizacoes de versao e mudancas de parametros usando a ferramenta `#editFiles`
1. **Validate**: Execute `bicep lint` e `bicep build` usando a ferramenta `#runCommands` para garantir conformidade.
1. **Output**: Resuma as mudancas em formato de tabela com um resumo das atualizacoes abaixo.

## Uso de Ferramentas

Sempre use as ferramentas `#search`, `#searchResults`, `#fetch`, `#editFiles`, `#runCommands`, `#todos` se estiverem disponiveis. Evite escrever codigo para executar tarefas.

## Politica de Mudancas com Quebra

⚠️ **PAUSE para aprovacao** se atualizacoes envolverem:

- Mudancas de parametros incompativeis
- Modificacoes de seguranca/conformidade
- Mudancas de comportamento

## Formato de Saida

Exiba resultados apenas em tabela com icones:

```markdown
| Module | Current | Latest | Status | Action | Docs |
|--------|---------|--------|--------|--------|------|
| avm/res/compute/vm | 0.1.0 | 0.2.0 | 🔄 | Updated | [📖](link) |
| avm/res/storage/account | 0.3.0 | 0.3.0 | ✅ | Current | [📖](link) |

### Summary of Updates

Describe updates made, any manual reviews needed or issues encountered.
```

## Icons

- 🔄 Updated
- ✅ Current
- ⚠️ Manual review required
- ❌ Failed
- 📖 Documentation

## Requirements

- Use MCR tags API only for version discovery
- Parse JSON tags array and sort by semantic versioning
- Maintain Bicep file validity and linting compliance
