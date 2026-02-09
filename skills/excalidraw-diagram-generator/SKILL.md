---
name: excalidraw-diagram-generator
description: 'Gere diagramas Excalidraw a partir de descricoes em linguagem natural. Use quando pedirem "criar um diagrama", "fazer um fluxograma", "visualizar um processo", "desenhar uma arquitetura de sistema", "criar um mind map" ou "gerar um arquivo Excalidraw". Suporta fluxogramas, diagramas de relacionamento, mind maps e diagramas de arquitetura. Gera arquivos JSON .excalidraw que podem ser abertos diretamente no Excalidraw.'
---

# Gerador de Diagramas Excalidraw

Uma skill para gerar diagramas no formato Excalidraw a partir de descricoes em linguagem natural. Esta skill ajuda a criar representacoes visuais de processos, sistemas, relacionamentos e ideias sem desenho manual.

## Quando Usar Esta Skill

Use esta skill quando usuarios solicitarem:

- "Crie um diagrama mostrando..."
- "Faca um fluxograma para..."
- "Visualize o processo de..."
- "Desenhe a arquitetura do sistema de..."
- "Gere um mind map sobre..."
- "Crie um arquivo Excalidraw para..."
- "Mostre a relacao entre..."
- "Faça um diagrama do workflow de..."

**Tipos de diagrama suportados:**
- 📊 **Fluxogramas**: Processos sequenciais, workflows, arvores de decisao
- 🔗 **Diagramas de Relacionamento**: Entidades, componentes de sistema, dependencias
- 🧠 **Mind Maps**: Hierarquias de conceitos, brainstorming, organizacao de topicos
- 🏗️ **Diagramas de Arquitetura**: Design de sistemas, interacoes de modulos, fluxo de dados
- 📈 **Diagramas de Fluxo de Dados (DFD)**: Visualizacao de fluxo de dados, processos de transformacao
- 🏊 **Business Flow (Swimlane)**: Workflows cross-funcionais, fluxos por ator
- 📦 **Diagramas de Classes**: Design OO, estruturas e relacionamentos de classes
- 🔄 **Diagramas de Sequencia**: Interacoes ao longo do tempo, fluxos de mensagens
- 🗃️ **Diagramas ER**: Relacionamentos entre entidades, modelos de dados

## Pre-requisitos

- Descricao clara do que deve ser visualizado
- Identificacao de entidades, etapas ou conceitos-chave
- Entendimento das relacoes ou fluxo entre elementos

## Fluxo de Trabalho Passo a Passo

### Etapa 1: Entender a Solicitação

Analise a descricao do usuario para determinar:
1. **Tipo de diagrama** (fluxograma, relacionamento, mind map, arquitetura)
2. **Elementos-chave** (entidades, etapas, conceitos)
3. **Relacionamentos** (fluxo, conexoes, hierarquia)
4. **Complexidade** (numero de elementos)

### Etapa 2: Escolher o Tipo de Diagrama Apropriado

| Intencao do Usuario | Tipo de Diagrama | Palavras-chave de Exemplo |
|-------------|--------------|------------------|
| Fluxo de processo, etapas, procedimentos | **Fluxograma** | "workflow", "process", "steps", "procedure" |
| Conexoes, dependencias, associacoes | **Diagrama de Relacionamento** | "relationship", "connections", "dependencies", "structure" |
| Hierarquia de conceitos, brainstorming | **Mind Map** | "mind map", "concepts", "ideas", "breakdown" |
| Design de sistema, componentes | **Diagrama de Arquitetura** | "architecture", "system", "components", "modules" |
| Fluxo de dados, processos de transformacao | **Data Flow Diagram (DFD)** | "data flow", "data processing", "data transformation" |
| Processos cross-funcionais, responsabilidades de atores | **Business Flow (Swimlane)** | "business process", "swimlane", "actors", "responsibilities" |
| Design OO, estruturas de classes | **Diagrama de Classes** | "class", "inheritance", "OOP", "object model" |
| Sequencias de interacao, fluxos de mensagens | **Diagrama de Sequencia** | "sequence", "interaction", "messages", "timeline" |
| Design de banco de dados, relacionamentos entre entidades | **Diagrama ER** | "database", "entity", "relationship", "data model" |

### Etapa 3: Extrair Informacoes Estruturadas

**Para Fluxogramas:**
- Lista de etapas sequenciais
- Pontos de decisao (se houver)
- Pontos de inicio e fim

**Para Diagramas de Relacionamento:**
- Entidades/nos (nome + descricao opcional)
- Relacionamentos entre entidades (de → para, com label)

**Para Mind Maps:**
- Topico central
- Ramos principais (3-6 recomendados)
- Sub-topicos por ramo (opcional)

**Para Diagramas de Fluxo de Dados (DFD):**
- Fontes e destinos de dados (entidades externas)
- Processos (transformacoes de dados)
- Data stores (bases, arquivos)
- Fluxos de dados (setas mostrando movimento de dados da esquerda para direita ou de cima-esquerda para baixo-direita)
- **Importante**: Nao representar ordem de processo, apenas fluxo de dados

**Para Business Flow (Swimlane):**
- Atores/roles (departamentos, sistemas, pessoas) - exibidos como colunas de cabecalho
- Lanes de processo (faixas verticais sob cada ator)
- Caixas de processo (atividades em cada lane)
- Setas de fluxo (conectando caixas, incluindo handoffs entre lanes)

**Para Diagramas de Classes:**
- Classes com nomes
- Atributos com visibilidade (+, -, #)
- Metodos com visibilidade e parametros
- Relacionamentos: heranca (linha solida + triangulo branco), implementacao (linha tracejada + triangulo branco), associacao (linha solida), dependencia (linha tracejada), agregacao (linha solida + losango branco), composicao (linha solida + losango preenchido)
- Notacoes de multiplicidade (1, 0..1, 1..*, *)

**Para Diagramas de Sequencia:**
- Objetos/atores (dispostos horizontalmente no topo)
- Lifelines (linhas verticais de cada objeto)
- Mensagens (setas horizontais entre lifelines)
- Mensagens sincronas (seta solida), assincronas (seta tracejada)
- Valores de retorno (setas tracejadas)
- Caixas de ativacao (retangulos nas lifelines durante execucao)
- O tempo flui de cima para baixo

**Para Diagramas ER:**
- Entidades (retangulos com nomes)
- Atributos (listados dentro das entidades)
- Chaves primarias (sublinhadas ou marcadas com PK)
- Chaves estrangeiras (marcadas com FK)
- Relacionamentos (linhas conectando entidades)
- Cardinalidade: 1:1 (um-para-um), 1:N (um-para-muitos), N:M (muitos-para-muitos)
- Entidades de juncao/associativas para relacionamentos muitos-para-muitos (retangulos tracejados)

### Etapa 4: Gerar o JSON do Excalidraw

Crie o arquivo `.excalidraw` com elementos apropriados:

**Tipos de elementos disponiveis:**
- `rectangle`: Caixas para entidades, etapas, conceitos
- `ellipse`: Formas alternativas para enfase
- `diamond`: Pontos de decisao
- `arrow`: Conexoes direcionais
- `text`: Labels e anotacoes

**Propriedades-chave a definir:**
- **Position**: Coordenadas `x`, `y`
- **Size**: `width`, `height`
- **Style**: `strokeColor`, `backgroundColor`, `fillStyle`
- **Font**: `fontFamily: 5` (Excalifont - **obrigatorio para todos os elementos de texto**)
- **Text**: Texto embutido nos labels
- **Connections**: Array `points` para setas

**Importante**: Todos os elementos de texto devem usar `fontFamily: 5` (Excalifont) para consistencia visual.

### Etapa 5: Formatar a Saida

Estruture o arquivo Excalidraw completo:

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    // Diagram elements array
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": 20
  },
  "files": {}
}
```

### Etapa 6: Salvar e Fornecer Instrucoes

1. Salve como `<descriptive-name>.excalidraw`
2. Informe ao usuario como abrir:
   - Visite https://excalidraw.com
   - Clique em "Open" ou arraste o arquivo
   - Ou use a extensao do VS Code para Excalidraw

## Boas Praticas

### Diretrizes de Contagem de Elementos

| Tipo de Diagrama | Contagem Recomendada | Maximo |
|--------------|-------------------|---------|
| Etapas de fluxograma | 3-10 | 15 |
| Entidades de relacionamento | 3-8 | 12 |
| Ramos de mind map | 4-6 | 8 |
| Sub-topicos por ramo | 2-4 | 6 |

### Dicas de Layout

1. **Posicoes iniciais**: Centralize elementos importantes, use espacamento consistente
2. **Espacamento**:
   - Gap horizontal: 200-300px entre elementos
   - Gap vertical: 100-150px entre linhas
3. **Cores**: Use esquema de cores consistente
   - Elementos primarios: Azul claro (`#a5d8ff`)
   - Elementos secundarios: Verde claro (`#b2f2bb`)
   - Importante/Central: Amarelo (`#ffd43b`)
   - Alertas/Avisos: Vermelho claro (`#ffc9c9`)
4. **Tamanho de texto**: 16-24px para legibilidade
5. **Fonte**: Sempre use `fontFamily: 5` (Excalifont) para todos os textos
6. **Estilo de setas**: Use setas retas para fluxos simples, curvas para relacionamentos complexos

### Gestao de Complexidade

**Se a solicitacao tiver muitos elementos:**
- Sugira dividir em multiplos diagramas
- Foque nos elementos principais primeiro
- Ofereca criar sub-diagramas detalhados

**Resposta de exemplo:**
```
"Your request includes 15 components. For clarity, I recommend:
1. High-level architecture diagram (6 main components)
2. Detailed diagram for each subsystem

Would you like me to start with the high-level view?"
```

## Exemplos de Prompts e Respostas

### Exemplo 1: Fluxograma Simples

**Usuario:** "Create a flowchart for user registration"

**Agente gera:**
1. Extrair etapas: "Enter email" → "Verify email" → "Set password" → "Complete"
2. Criar fluxograma com 4 retangulos + 3 setas
3. Salvar como `user-registration-flow.excalidraw`

### Exemplo 2: Diagrama de Relacionamento

**Usuario:** "Diagram the relationship between User, Post, and Comment entities"

**Agente gera:**
1. Entidades: User, Post, Comment
2. Relacionamentos: User → Post ("creates"), User → Comment ("writes"), Post → Comment ("contains")
3. Salvar como `user-content-relationships.excalidraw`

### Exemplo 3: Mind Map

**Usuario:** "Mind map about machine learning concepts"

**Agente gera:**
1. Centro: "Machine Learning"
2. Ramos: Supervised Learning, Unsupervised Learning, Reinforcement Learning, Deep Learning
3. Sub-topicos em cada ramo
4. Salvar como `machine-learning-mindmap.excalidraw`

## Solucao de Problemas

| Problema (Issue) | Solucao |
|-------|----------|
| Elementos se sobrepoem | Aumente o espacamento entre coordenadas |
| Texto nao cabe em caixas | Aumente a largura da caixa ou reduza o tamanho da fonte |
| Muitos elementos | Divida em multiplos diagramas |
| Layout pouco claro | Use grid layout (linhas/colunas) ou layout radial (mind maps) |
| Cores inconsistentes | Defina paleta de cores upfront baseada em tipos de elemento |

## Tecnicas Avancadas

### Grid Layout (para Diagramas de Relacionamento)
```javascript
const columns = Math.ceil(Math.sqrt(entityCount));
const x = startX + (index % columns) * horizontalGap;
const y = startY + Math.floor(index / columns) * verticalGap;
```

### Radial Layout (para Mind Maps)
```javascript
const angle = (2 * Math.PI * index) / branchCount;
const x = centerX + radius * Math.cos(angle);
const y = centerY + radius * Math.sin(angle);
```

### IDs Auto-gerados
Use timestamp + string randomica para IDs unicos:
```javascript
const id = Date.now().toString(36) + Math.random().toString(36).substr(2);
```

## Formato de Saida (Output)

Sempre forneca:
1. ✅ Arquivo JSON `.excalidraw` completo
2. 📊 Resumo do que foi criado
3. 📝 Contagem de elementos
4. 💡 Instrucoes para abrir/editar

**Resumo de exemplo:**
```
Created: user-workflow.excalidraw
Type: Flowchart
Elements: 7 rectangles, 6 arrows, 1 title text
Total: 14 elements

To view:
1. Visit https://excalidraw.com
2. Drag and drop user-workflow.excalidraw
3. Or use File → Open in Excalidraw VS Code extension
```

## Checklist de Validacao

Antes de entregar o diagrama:
- [ ] Todos os elementos possuem IDs unicos
- [ ] Coordenadas evitam sobreposicao
- [ ] Texto legivel (font size 16+)
- [ ] **Todos os elementos de texto usam `fontFamily: 5` (Excalifont)**
- [ ] Setas conectam logicamente
- [ ] Cores seguem esquema consistente
- [ ] Arquivo e JSON valido
- [ ] Contagem de elementos e razoavel (<20 para clareza)

## Bibliotecas de Icones (Aprimoramento Opcional)

Para diagramas especializados (ex.: AWS/GCP/Azure), voce pode usar bibliotecas de icones prontas do Excalidraw. Isso fornece icones profissionais e padronizados em vez de formas basicas.

### Quando o Usuario Solicita Icones

**Se o usuario pedir diagramas de arquitetura AWS/cloud ou mencionar uso de icones especificos:**

1. **Verifique se a biblioteca existe**: Procure `libraries/<library-name>/reference.md`
2. **Se a biblioteca existir**: Prossiga usando icones (veja o workflow de AI Assistant abaixo)
3. **Se a biblioteca NAO existir**: Responda com instrucoes de setup:

   ```
   To use [AWS/GCP/Azure/etc.] architecture icons, please follow these steps:
   
   1. Visit https://libraries.excalidraw.com/
   2. Search for "[AWS Architecture Icons/etc.]" and download the .excalidrawlib file
   ```
