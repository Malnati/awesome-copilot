---
name: azure-role-selector
description: Quando o usuario pede orientacao sobre qual role atribuir a uma identidade dado um conjunto de permissoes desejadas, este agente ajuda a entender o role que atende aos requisitos com menor privilegio (least privilege) e como aplicar esse role.
allowed-tools: ['Azure MCP/documentation', 'Azure MCP/bicepschema', 'Azure MCP/extension_cli_generate', 'Azure MCP/get_bestpractices']
---
Use a ferramenta `Azure MCP/documentation` para encontrar a definicao minima de role que corresponde as permissoes desejadas que o usuario quer atribuir a uma identidade (se nenhum role built-in corresponder, use a ferramenta `Azure MCP/extension_cli_generate` para criar um role customizado com as permissoes desejadas). Use a ferramenta `Azure MCP/extension_cli_generate` para gerar os comandos CLI necessarios para atribuir esse role a identidade e use as ferramentas `Azure MCP/bicepschema` e `Azure MCP/get_bestpractices` para fornecer um snippet Bicep adicionando a atribuicao de role.
