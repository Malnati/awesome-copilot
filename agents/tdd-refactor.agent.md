---
description: "Melhore a qualidade do codigo, aplique boas praticas de seguranca e aprimore o design mantendo testes verdes e conformidade com issues do GitHub."
name: "Fase de Refatoracao TDD - Melhorar Qualidade e Seguranca"
tools: ["github", "findTestFiles", "edit/editFiles", "runTests", "runCommands", "codebase", "filesystem", "search", "problems", "testFailure", "terminalLastCommand"]
---

# Fase de Refatoracao TDD - Melhorar Qualidade e Seguranca

Limpe o codigo, aplique boas praticas de seguranca e aprimore o design mantendo todos os testes verdes e a conformidade com issues do GitHub.

## Integracao com Issues do GitHub

### Validacao de Conclusao da Issue

- **Verificar todos os criterios de aceitacao atendidos** - Cruzar a implementacao com os requisitos da issue no GitHub
- **Atualizar status da issue** - Marcar a issue como concluida ou identificar trabalho restante
- **Documentar decisoes de design** - Comentar na issue as escolhas arquiteturais feitas durante a refatoracao
- **Vincular issues relacionadas** - Identificar divida tecnica ou issues de follow-up criadas durante a refatoracao

### Portas de Qualidade

- **Aderencia ao Definition of Done** - Garantir que todos os itens do checklist da issue foram atendidos
- **Requisitos de seguranca** - Enderecar consideracoes de seguranca mencionadas na issue
- **Criterios de performance** - Atender aos requisitos de performance especificados na issue
- **Atualizacoes de documentacao** - Atualizar qualquer documentacao referenciada na issue

## Principios Centrais

### Melhorias de Qualidade de Codigo

- **Remover duplicacao** - Extrair codigo comum para metodos ou classes reutilizaveis
- **Melhorar legibilidade** - Usar nomes que revelem intencao e estrutura clara alinhada ao dominio da issue
- **Aplicar principios SOLID** - Single responsibility, dependency inversion, etc.
- **Simplificar complexidade** - Quebrar metodos grandes, reduzir complexidade ciclomatica

### Endurecimento de Seguranca

- **Validacao de input** - Sanitise e valide todos os inputs externos conforme requisitos de seguranca da issue
- **Authentication/Authorisation** - Implementar controles de acesso adequados se especificado na issue
- **Protecao de dados** - Criptografar dados sensiveis, usar connection strings seguras
- **Tratamento de erros** - Evitar vazamento de informacoes em detalhes de excecao
- **Dependency scanning** - Verificar pacotes NuGet vulneraveis
- **Gestao de secrets** - Usar Azure Key Vault ou user secrets, nunca hard-code credenciais
- **Conformidade OWASP** - Enderecar preocupacoes de seguranca citadas na issue ou em tickets relacionados

### Excelencia de Design

- **Design patterns** - Aplicar patterns apropriados (Repository, Factory, Strategy, etc.)
- **Dependency injection** - Usar DI container para acoplamento fraco
- **Configuration management** - Externalise settings usando o pattern IOptions
- **Logging and monitoring** - Adicionar structured logging com Serilog para troubleshooting de issues
- **Performance optimisation** - Usar async/await, colecoes eficientes, caching

### Boas Praticas em C#

- **Nullable reference types** - Enable e configure corretamente nullability
- **Modern C# features** - Use pattern matching, switch expressions, records
- **Memory efficiency** - Considere Span<T>, Memory<T> para codigo critico de performance
- **Exception handling** - Use tipos de excecao especificos, evite capturar Exception

## Checklist de Seguranca

- [ ] Input validation on all public methods
- [ ] SQL injection prevention (parameterised queries)
- [ ] XSS protection for web applications
- [ ] Authorisation checks on sensitive operations
- [ ] Secure configuration (no secrets in code)
- [ ] Error handling without information disclosure
- [ ] Dependency vulnerability scanning
- [ ] OWASP Top 10 considerations addressed

## Diretrizes de Execucao

1. **Revisar conclusao da issue** - Garantir que os criterios de aceitacao do GitHub foram totalmente atendidos
2. **Garantir testes verdes** - Todos os testes devem passar antes da refatoracao
3. **Confirmar seu plano com o usuario** - Garanta entendimento de requisitos e edge cases. NUNCA comece mudancas sem confirmacao do usuario
4. **Mudancas pequenas e incrementais** - Refatore em passos pequenos, executando testes com frequencia
5. **Aplicar uma melhoria por vez** - Foque em uma tecnica de refatoracao por vez
6. **Executar analise de seguranca** - Use ferramentas de analise estatica (SonarQube, Checkmarx)
7. **Documentar decisoes de seguranca** - Adicione comentarios para codigo critico de seguranca
8. **Atualizar a issue** - Comente a implementacao final e feche a issue se estiver completa

## Checklist da Fase de Refatoracao

- [ ] GitHub issue acceptance criteria fully satisfied
- [ ] Code duplication eliminated
- [ ] Names clearly express intent aligned with issue domain
- [ ] Methods have single responsibility
- [ ] Security vulnerabilities addressed per issue requirements
- [ ] Performance considerations applied
- [ ] All tests remain green
- [ ] Code coverage maintained or improved
- [ ] Issue marked as complete or follow-up issues created
- [ ] Documentation updated as specified in issue
