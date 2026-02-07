# DFRobot FIT0127 LCD Character Display Specification

## 1. Visao geral

O **DFRobot FIT0127** e uma familia de **modulos de LCD de caracteres compativeis com HD44780**, comumente usados em sistemas embarcados e projetos hobby. Esses displays fornecem saida alfanumerica usando um gerador de caracteres dot-matrix e suportam interfaces paralelas 8-bit e 4-bit.

Os modulos FIT0127 sao frequentemente combinados com microcontroladores e CPUs 8-bit como **6502**, **AVR**, **PIC** e plataformas **Arduino**.

---

## 2. Caracteristicas gerais

| Recurso            | Descricao                                           |
| ------------------ | --------------------------------------------------- |
| Tipo de display    | Character LCD (STN, Yellow-Green backlight typical) |
| Controlador        | HD44780 ou compativel                               |
| Interface          | Parallel (4-bit or 8-bit)                           |
| Tamanho caractere  | 5 x 8 dot matrix                                    |
| Tensao operacao    | Logica 5 V (algumas variantes suportam 3.3 V)       |
| Backlight          | LED (pinos de energia separados)                   |
| Modo de visualizacao | Transflective                                    |

---

## 3. Variantes do display

A designacao FIT0127 e comumente associada a **modulos LCD de caracteres 16x2**.

| Variante | Caracteres         |
| -------- | ------------------ |
| FIT0127  | 16 colunas x 2 linhas |

---

## 4. Configuracao de pinos

### 4.1 Pinout (Header padrao 16 pinos)

| Pino | Nome | Descricao              |
| --:  | ---- | ---------------------- |
|   1  | VSS  | Ground                 |
|   2  | VDD  | Alimentacao +5 V       |
|   3  | VO   | Ajuste de contraste    |
|   4  | RS   | Register Select        |
|   5  | R/W  | Read/Write select      |
|   6  | E    | Enable                 |
|   7  | D0   | Data bit 0             |
|   8  | D1   | Data bit 1             |
|   9  | D2   | Data bit 2             |
|  10  | D3   | Data bit 3             |
|  11  | D4   | Data bit 4             |
|  12  | D5   | Data bit 5             |
|  13  | D6   | Data bit 6             |
|  14  | D7   | Data bit 7             |
|  15  | A    | Backlight Anode (+)    |
|  16  | K    | Backlight Cathode (-)  |

---

## 5. Caracteristicas eletricas (tipicas)

| Parametro          | Valor       |
| ------------------ | ----------- |
| Tensao logica (VDD) | 4.5 - 5.5 V |
| Corrente logica    | ~1-2 mA     |
| Tensao backlight   | ~4.2 V      |
| Corrente backlight | 15-30 mA    |

---

## 6. Sinais de interface

### 6.1 RS (Register Select)

| RS | Funcao               |
| -- | -------------------- |
| 0  | Instruction register |
| 1  | Data register        |

### 6.2 R/W

| R/W | Operacao       |
| --- | -------------- |
| 0   | Write to LCD   |
| 1   | Read from LCD  |

### 6.3 Enable (E)

* Dados sao capturados na **borda de descida** de E
* E deve ser pulsado HIGH -> LOW para cada transferencia

---

## 7. Operacao do barramento de dados

### 7.1 Modo 8-bit

* Usa D0-D7
* Operacao mais rapida

### 7.2 Modo 4-bit

* Usa apenas D4-D7
* Dados transferidos em dois nibbles (high primeiro)
* Economiza pinos de I/O

---

## 8. Mapa de memoria interno

### 8.1 DDRAM (Display Data RAM)

| Endereco | Posicao no display |
| -------: | ------------------ |
| 0x00-0x0F | Linha 1, Col 1-16 |
| 0x40-0x4F | Linha 2, Col 1-16 |

### 8.2 CGRAM (Character Generator RAM)

* Suporta ate **8 caracteres customizados**
* Cada caractere usa 8 bytes

---

## 9. Conjunto de instrucoes (resumo)

| Instrucao | Descricao               |
| --------- | ----------------------- |
| 0x01      | Clear display           |
| 0x02      | Return home             |
| 0x04-0x07 | Entry mode set          |
| 0x08-0x0F | Display on/off control  |
| 0x10-0x1F | Cursor/display shift    |
| 0x20-0x3F | Function set            |
| 0x40-0x7F | Set CGRAM address       |
| 0x80-0xFF | Set DDRAM address       |

---

## 10. Sequencia de inicializacao (modo 4-bit)

```text
Wait >15 ms after VDD rises
Function set: 0x33
Function set: 0x32
Function set: 0x28 (4-bit, 2-line)
Display ON/OFF: 0x0C
Entry mode: 0x06
Clear display: 0x01
```

---

## 11. Caracteristicas de timing (tipicas)

| Operacao            | Tempo    |
| ------------------- | -------- |
| Enable pulse width  | >= 450 ns |
| Command execution   | ~37 us   |
| Clear / Home        | ~1.52 ms |

---

## 12. Integracao tipica no sistema (exemplo 6502)

```text
RS  -> VIA output
E   -> VIA output
D4-D7 -> VIA Port B
R/W -> Grounded (write-only)
```

---

## 13. Controle de contraste e backlight

* Contraste ajustado via potenciometro no pino VO
* Backlight pode exigir resistor em serie
* PWM dimming suportado via controle externo

---

## 14. Ratings maximos absolutos (resumo)

| Parametro       | Rating                |
| --------------- | --------------------- |
| VDD             | -0.3 V to +6.0 V      |
| Input voltage   | -0.3 V to VDD + 0.3 V |
| Operating temp  | -20 C to +70 C        |

---

## 15. Casos de uso comuns

* Status displays
* Debug output para SBCs
* User interfaces para sistemas embarcados
* Retrocomputer front panels

---

## 16. Referencias

* <https://static6.arrow.com/aropdfconversion/1f68489996f057bb6611f71d5fdb5f60f44faa72/pgurl_58439499065092.pdf>
* <https://cdn.sparkfun.com/assets/9/5/f/7/b/HD44780.pdf>
* <https://predictabledesigns.com/introduction-embedded-electronic-displays/>

---
