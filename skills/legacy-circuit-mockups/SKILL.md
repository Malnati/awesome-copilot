---
name: legacy-circuit-mockups
description: 'Gere mockups de circuitos em breadboard e diagramas visuais usando tecnicas de desenho em HTML5 Canvas. Use quando pedirem layouts de circuitos, visualizacao de posicionamento de componentes, diagramas de breadboard, mockup de builds 6502, esquemas de computadores retro ou projetos de eletronica vintage. Suporta timers 555, microprocessadores W65C02S, EEPROMs 28C256, chips W65C22 VIA, portas logicas serie 7400, LEDs, resistores, capacitores, chaves, botoes, cristais e fios.'
---

# Mockups de Circuitos Legacy

Uma skill para criar mockups de circuitos em breadboard e diagramas visuais para projetos de computacao retro e eletronica. Esta skill utiliza mecanismos de desenho em HTML5 Canvas para renderizar layouts de circuitos interativos com componentes vintage como microprocessador 6502, CIs timer 555, EEPROMs e portas logicas serie 7400.

## Quando Usar Esta Skill

- Usuario pede para "criar um layout de breadboard" ou "mockupar um circuito"
- Usuario quer visualizar posicionamento de componentes em um breadboard
- Usuario precisa de referencia visual para montar um computador 6502
- Usuario pede para "desenhar um circuito" ou "diagramar eletronica"
- Usuario quer criar visuais educacionais de eletronica
- Usuario menciona tutoriais do Ben Eater ou projetos de computacao retro
- Usuario pede mockup de circuitos com timer 555 ou projetos com LED
- Usuario precisa visualizar conexoes de fios entre componentes

## Pre-requisitos

- Entendimento de pinouts de componentes a partir dos arquivos de referencia incluidos
- Conhecimento de convencoes de layout de breadboard (linhas, colunas, trilhos de alimentacao)

## Componentes Suportados

### Microprocessadores e Memoria

| Componente | Pinos | Descricao |
|-----------|------|-------------|
| W65C02S | DIP de 40 pinos | Microprocessador 8-bit com barramento de endereco 16-bit |
| 28C256 | DIP de 28 pinos | EEPROM paralela 32KB |
| W65C22 | DIP de 40 pinos | Versatile Interface Adapter (VIA) |
| 62256 | DIP de 28 pinos | RAM estatica 32KB |

### CIs de Logica e Timer

| Componente | Pinos | Descricao |
|-----------|------|-------------|
| NE555 | DIP de 8 pinos | CI timer para temporizacao e oscilacao |
| 7400 | DIP de 14 pinos | Quad NAND 2 entradas |
| 7402 | DIP de 14 pinos | Quad NOR 2 entradas |
| 7404 | DIP de 14 pinos | Hex inverter (porta NOT) |
| 7408 | DIP de 14 pinos | Quad AND 2 entradas |
| 7432 | DIP de 14 pinos | Quad OR 2 entradas |

### Componentes Passivos e Ativos

| Componente | Descricao |
|-----------|-------------|
| LED | Diodo emissor de luz (varias cores) |
| Resistor | Limitacao de corrente (valores configuraveis) |
| Capacitor | Filtragem e temporizacao (ceramico/eletrolitico) |
| Crystal | Oscilador de clock |
| Switch | Chave de alternancia (latching) |
| Button | Botao momentaneo |
| Potentiometer | Resistor variavel |
| Photoresistor | Resistor dependente de luz |

### Sistema de Grid

```javascript
// Standard breadboard grid: 20px spacing
const gridSize = 20;
const cellX = Math.floor(x / gridSize) * gridSize;
const cellY = Math.floor(y / gridSize) * gridSize;
```

### Padrao de Renderizacao de Componentes

```javascript
// All components follow this structure:
{
  type: 'component-type',
  x: gridX,
  y: gridY,
  width: componentWidth,
  height: componentHeight,
  rotation: 0,  // 0, 90, 180, 270
  properties: { /* component-specific data */ }
}
```

### Conexoes de Fios

```javascript
// Wire connection format:
{
  start: { x: startX, y: startY },
  end: { x: endX, y: endY },
  color: '#ff0000'  // Wire color coding
}
```

## Fluxos de Trabalho Passo a Passo

### Criar um Mockup de Circuito LED Basico

1. Defina dimensoes do breadboard e grid
2. Posicione conexoes nos trilhos de alimentacao (+5V e GND)
3. Adicione o componente LED com orientacao anodo/catodo
4. Posicione o resistor limitador de corrente
5. Desenhe as conexoes de fio entre componentes
6. Adicione labels e anotacoes

### Criar um Circuito com Timer 555

1. Posicione o CI NE555 no breadboard (pinos 1-4 a esquerda, 5-8 a direita)
2. Conecte o pino 1 (GND) ao trilho de ground
3. Conecte o pino 8 (Vcc) ao trilho de alimentacao
4. Adicione resistores e capacitores de temporizacao
5. Conecte trigger e threshold
6. Conecte a saida ao LED ou outra carga

### Criar um Layout de Microprocessador 6502

1. Posicione o W65C02S centralizado no breadboard
2. Adicione EEPROM 28C256 para armazenamento de programa
3. Posicione W65C22 VIA para I/O
