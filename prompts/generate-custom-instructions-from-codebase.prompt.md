---
description: 'Gerador de instrucoes de migracao e evolucao de codigo para GitHub Copilot. Analisa diferencas entre duas versoes do projeto (branches, commits ou releases) para criar instrucoes precisas que permitem ao Copilot manter consistencia durante migracoes de tecnologia, grandes refatoracoes ou upgrades de frameworks.'
agent: 'agent'
---

# Gerador de Instrucoes de Migracao e Evolucao de Codigo

## Variaveis de Configuracao

```
${MIGRATION_TYPE="Framework Version|Architecture Refactoring|Technology Migration|Dependencies Update|Pattern Changes"}
<!-- Tipo de migracao ou evolucao -->

${SOURCE_REFERENCE="branch|commit|tag"}
<!-- Ponto de referencia de origem (estado anterior) -->

${TARGET_REFERENCE="branch|commit|tag"}  
<!-- Ponto de referencia de destino (estado posterior) -->

${ANALYSIS_SCOPE="Entire project|Specific folder|Modified files only"}
<!-- Escopo de analise -->

${CHANGE_FOCUS="Breaking Changes|New Conventions|Obsolete Patterns|API Changes|Configuration"}
<!-- Aspecto principal das mudancas -->

${AUTOMATION_LEVEL="Conservative|Balanced|Aggressive"}
<!-- Nivel de automacao para sugestoes do Copilot -->

${GENERATE_EXAMPLES="true|false"}
<!-- Incluir exemplos de transformacao -->

${VALIDATION_REQUIRED="true|false"}
<!-- Exigir validacao antes de aplicar -->
```

## Prompt Gerado

```
"Analise a evolucao do codigo entre dois estados do projeto para gerar instrucoes de migracao precisas para GitHub Copilot. Essas instrucoes vao orientar o Copilot a aplicar os mesmos patterns de transformacao durante modificacoes futuras. Siga esta metodologia:

### Phase 1: Comparative State Analysis

#### Structural Changes Detection
- Compare a estrutura de pastas entre ${SOURCE_REFERENCE} e ${TARGET_REFERENCE}
- Identifique arquivos movidos, renomeados ou deletados
- Analise mudancas em arquivos de configuracao
- Documente novas dependencias e dependencias removidas

#### Code Transformation Analysis
${MIGRATION_TYPE == "Framework Version" ? 
  "- Identifique mudancas de API entre versoes do framework
   - Analise novas features em uso
   - Documente metodos/propriedades obsoletos
   - Observe mudancas de sintaxe ou convencao" : ""}

${MIGRATION_TYPE == "Architecture Refactoring" ? 
  "- Analise mudancas de pattern arquitetural
   - Identifique novas abstracoes introduzidas
   - Documente reorganizacao de responsabilidades
   - Observe mudancas nos fluxos de dados" : ""}

${MIGRATION_TYPE == "Technology Migration" ? 
  "- Analise substituicao de uma tecnologia por outra
   - Identifique equivalencias funcionais
   - Documente mudancas de API e sintaxe
   - Observe novas dependencias e configuracoes" : ""}

#### Transformation Pattern Extraction
- Identifique transformacoes repetitivas aplicadas
- Analise regras de conversao do formato antigo para o novo
- Documente excecoes e casos especiais
- Crie matriz de correspondencia antes/depois

### Phase 2: Migration Instructions Generation

Crie um arquivo `.github/copilot-migration-instructions.md` com a seguinte estrutura:

\`\`\`markdown
# GitHub Copilot Migration Instructions

## Migration Context
- **Type**: ${MIGRATION_TYPE}
- **From**: ${SOURCE_REFERENCE} 
- **To**: ${TARGET_REFERENCE}
- **Date**: [GENERATION_DATE]
- **Scope**: ${ANALYSIS_SCOPE}

## Automatic Transformation Rules

### 1. Mandatory Transformations
${AUTOMATION_LEVEL != "Conservative" ? 
  "[AUTOMATIC_TRANSFORMATION_RULES]
   - **Old Pattern**: [OLD_CODE]
   - **New Pattern**: [NEW_CODE]
   - **Trigger**: When to detect this pattern
   - **Action**: Transformation to apply automatically" : ""}

### 2. Transformations with Validation
${VALIDATION_REQUIRED == "true" ? 
  "[TRANSFORMATIONS_WITH_VALIDATION]
   - **Detected Pattern**: [DESCRIPTION]
   - **Suggested Transformation**: [NEW_APPROACH]
   - **Required Validation**: [VALIDATION_CRITERIA]
   - **Alternatives**: [ALTERNATIVE_OPTIONS]" : ""}

### 3. API Correspondences
${CHANGE_FOCUS == "API Changes" || MIGRATION_TYPE == "Framework Version" ? 
  "[API_CORRESPONDENCE_TABLE]
   | Old API   | New API   | Notes     | Example        |
   | --------- | --------- | --------- | -------------- |
   | [OLD_API] | [NEW_API] | [CHANGES] | [CODE_EXAMPLE] | " : ""} |

### 4. New Patterns to Adopt
[DETECTED_EMERGING_PATTERNS]
- **Pattern**: [PATTERN_NAME]
- **Usage**: [WHEN_TO_USE] 
- **Implementation**: [HOW_TO_IMPLEMENT]
- **Benefits**: [ADVANTAGES]

### 5. Obsolete Patterns to Avoid
[DETECTED_OBSOLETE_PATTERNS]
- **Obsolete Pattern**: [OLD_PATTERN]
- **Why Avoid**: [REASONS]
- **Alternative**: [NEW_PATTERN]
- **Migration**: [CONVERSION_STEPS]

## File Type Specific Instructions

${GENERATE_EXAMPLES == "true" ? 
  "### Configuration Files
   [CONFIG_TRANSFORMATION_EXAMPLES]
   
   ### Main Source Files
   [SOURCE_TRANSFORMATION_EXAMPLES]
   
   ### Test Files
   [TEST_TRANSFORMATION_EXAMPLES]" : ""}

## Validation and Security

### Automatic Control Points
- Verificacoes apos cada transformacao
- Testes a rodar para validar mudancas
- Metricas de performance a monitorar
- Checks de compatibilidade

### Manual Escalation
Situacoes que exigem intervencao humana:
- [COMPLEX_CASES_LIST]
- [ARCHITECTURAL_DECISIONS]
- [BUSINESS_IMPACTS]

## Migration Monitoring

### Tracking Metrics
- Percentual de codigo migrado automaticamente
- Numero de validacoes manuais necessarias
- Taxa de erro das transformacoes automaticas
- Tempo medio de migracao por arquivo

### Error Reporting
Como reportar transformacoes incorretas ao Copilot:
- Patterns de feedback para melhorar regras
- Excecoes a documentar
- Ajustes a fazer nas instrucoes

\`\`\`

### Phase 3: Contextual Examples Generation

${GENERATE_EXAMPLES == "true" ? 
  "#### Transformation Examples
   Para cada pattern identificado, gere:
   
   \`\`\`
   // BEFORE (${SOURCE_REFERENCE})
   [OLD_CODE_EXAMPLE]
   
   // AFTER (${TARGET_REFERENCE}) 
   [NEW_CODE_EXAMPLE]
   
   // COPILOT INSTRUCTIONS
   When you see this pattern [TRIGGER], transform it to [NEW_PATTERN] following these steps: [STEPS]
   \`\`\`" : ""}

### Phase 4: Validation and Optimization

#### Instructions Testing
- Aplique instrucoes em codigo de teste
- Verifique consistencia das transformacoes
- Ajuste regras com base em resultados
- Documente excecoes e edge cases

#### Iterative Optimization  
${AUTOMATION_LEVEL == "Aggressive" ? 
  "- Refine rules to maximize automation
   - Reduce false positives in detection
   - Improve transformation accuracy
   - Document lessons learned" : ""}

### Final Result

Instrucoes de migracao que permitem ao GitHub Copilot:
1. **Aplicar automaticamente** as mesmas transformacoes em modificacoes futuras
2. **Manter consistencia** com novas convencoes adotadas  
3. **Evitar patterns obsoletos** propondo alternativas automaticamente
4. **Acelerar migracoes futuras** capitalizando experiencia adquirida
5. **Reduzir erros** automatizando transformacoes repetitivas

Essas instrucoes transformam o Copilot em um assistente inteligente de migracao, capaz de reproduzir decisoes de evolucao tecnologica de forma consistente e confiavel.
"
```

## Casos de Uso Tipicos

### Migracao de Versao de Framework
Perfeito para documentar transicao de Angular 14 para Angular 17, React Class Components para Hooks, ou .NET Framework para .NET Core. Identifica automaticamente breaking changes e gera regras de transformacao correspondentes.

### Evolucao de Stack Tecnologica  
Essencial ao substituir uma tecnologia inteira: jQuery para React, REST para GraphQL, SQL para NoSQL. Cria um guia de migracao abrangente com mapeamentos de patterns.

### Refatoracao de Arquitetura
Ideal para grandes refatoracoes como Monolith para Microservices, MVC para Clean Architecture ou Component para Composable architecture. Preserva conhecimento arquitetural para transformacoes futuras similares.

### Modernizacao de Design Patterns
Util para adotar novos patterns: Repository Pattern, Dependency Injection, Observer para Reactive Programming. Documenta racional e diferencas de implementacao.

## Beneficios Unicos

### 🧠 **Artificial Intelligence Enhancement**
Diferente de documentacao tradicional de migracao, estas instrucoes "treinam" o GitHub Copilot a reproduzir decisoes de evolucao tecnologica automaticamente em futuras modificacoes.

### 🔄 **Knowledge Capitalization**  
Transforma experiencia especifica do projeto em regras reutilizaveis, evitando perda de expertise e acelerando migracoes futuras similares.

### 🎯 **Context-Aware Precision**
Em vez de conselhos genericos, gera instrucoes sob medida para seu codebase, com exemplos reais de before/after.

### ⚡ **Automated Consistency**
Garante que novas adicoes de codigo sigam automaticamente as novas convencoes, prevenindo regressao arquitetural e mantendo coerencia evolutiva.
