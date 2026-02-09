---
agent: 'agent'
description: 'Revise codigo C#/.NET para implementacao de design patterns e sugira melhorias.'
---
# Revisao de Design Patterns em .NET/C#

Revise o codigo C#/.NET em ${selection} para implementacao de design patterns e sugira melhorias para o solution/projeto. Nao faca alteracoes no codigo, apenas forneca uma revisao.

## Design Patterns Obrigatorios

- **Command Pattern**: Classes base genericas (`CommandHandler<TOptions>`), interface `ICommandHandler<TOptions>`, heranca de `CommandHandlerOptions`, metodos estaticos `SetupCommand(IHost host)`
- **Factory Pattern**: Criacao de objetos complexos com integracao ao service provider
- **Dependency Injection**: Sintaxe de primary constructor, null checks com `ArgumentNullException`, abstracoes por interface, lifetimes corretos
- **Repository Pattern**: Interfaces de acesso async a dados e abstracoes de provider para conexoes
- **Provider Pattern**: Abstracoes de servicos externos (banco, IA), contratos claros, configuracao
- **Resource Pattern**: ResourceManager para mensagens localizadas, arquivos .resx separados (LogMessages, ErrorMessages)

## Checklist de Revisao

- **Design Patterns**: Identificar patterns usados. Command Handler, Factory, Provider e Repository estao corretos? Faltam patterns beneficiosos?
- **Arquitetura**: Segue convencoes de namespace (`{Core|Console|App|Service}.{Feature}`)? Separacao adequada entre projetos Core/Console? Modular e legivel?
- **Boas Praticas .NET**: Primary constructors, async/await com Task, ResourceManager, logging estruturado, configuracao fortemente tipada?
- **GoF Patterns**: Command, Factory, Template Method, Strategy corretamente implementados?
- **Principios SOLID**: Violacoes de Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion?
- **Performance**: async/await adequado, disposal de recursos, ConfigureAwait(false), oportunidades de paralelizacao?
- **Maintainability**: Separacao clara de responsabilidades, tratamento de erro consistente, configuracao correta?
- **Testabilidade**: Dependencias abstraidas por interfaces, componentes mockaveis, testabilidade async, compatibilidade com AAA?
- **Seguranca**: Validacao de input, credenciais seguras, queries parametrizadas, excecoes seguras?
- **Documentacao**: XML docs para APIs publicas, descricoes de parametros/retornos, organizacao de resource files?
- **Clareza**: Nomes significativos e alinhados ao dominio, intencao clara pelos patterns, estrutura autoexplicativa?
- **Clean Code**: Estilo consistente, tamanho adequado de metodos/classes, complexidade minima, duplicacao eliminada?

## Areas de Foco para Melhorias

- **Command Handlers**: Validacao na base, tratamento de erro consistente, gerenciamento adequado de recursos
- **Factories**: Configuracao de dependencias, integracao ao service provider, patterns de disposal
- **Providers**: Gerenciamento de conexao, patterns async, tratamento de excecoes e logging
- **Configuracao**: Data annotations, atributos de validacao, manuseio seguro de valores sensiveis
- **Integracao de IA/ML**: Patterns de Semantic Kernel, handling de output estruturado, configuracao de modelo

Forneca recomendacoes especificas e acionaveis alinhadas com a arquitetura do projeto e boas praticas .NET.
