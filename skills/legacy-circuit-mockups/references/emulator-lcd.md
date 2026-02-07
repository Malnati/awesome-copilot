# DFRobot FIT0127 LCD Character Display Emulation Specification

## Visao geral

Este documento especifica como **emular o modulo DFRobot FIT0127 LCD Character Display**. O FIT0127 e um **LCD de caracteres 16x2** compativel com o **controlador HD44780**, comumente usado em sistemas breadboard 6502 e microcontroladores.

O objetivo e **correcao funcional** para emuladores de SBC, testes de firmware e visualizacao de UI, em vez de simulacao de sinais eletricos.

---

## Resumo do modulo

| Recurso          | Valor                   |
| ---------------- | ----------------------- |
| Tipo de display  | Character LCD           |
| Resolucao        | 16 colunas x 2 linhas   |
| Controlador      | HD44780-compatible      |
| Interface        | 8-bit or 4-bit parallel |
| Matriz de caract | 5x8 dots                |
| Tensao de alim.  | 5V                      |

---

## Definicoes de pinos

| Pino | Nome  | Funcao           |
| ---- | ----- | ---------------- |
| 1    | GND   | Ground           |
| 2    | VCC   | +5V              |
| 3    | VO    | Contrast control |
| 4    | RS    | Register Select  |
| 5    | R/W   | Read / Write     |
| 6    | E     | Enable           |
| 7-14 | D0-D7 | Data bus         |
| 15   | A     | Backlight +      |
| 16   | K     | Backlight -      |

---

## Registradores logicos

| RS | Registrador          |
| -- | -------------------- |
| 0  | Instruction Register |
| 1  | Data Register        |

---

## Conjunto de instrucoes (subconjunto)

| Comando         | Codigo      | Descricao                 |
| --------------- | ----------- | ------------------------- |
| Clear Display   | `0x01`      | Clears DDRAM, cursor home |
| Return Home     | `0x02`      | Cursor to home position   |
| Entry Mode Set  | `0x04-0x07` | Cursor direction          |
| Display Control | `0x08-0x0F` | Display, cursor, blink    |
| Cursor/Shift    | `0x10-0x1F` | Shift cursor/display      |
| Function Set    | `0x20-0x3F` | Data length, lines        |
| Set CGRAM Addr  | `0x40-0x7F` | Custom chars              |
| Set DDRAM Addr  | `0x80-0xFF` | Cursor position           |

---

## Modelo de memoria interno

### DDRAM (Display Data RAM)

* Tamanho: 80 bytes
* Base linha 1: `0x00`
* Base linha 2: `0x40`

Mapeamento no emulador:

```text
Row 0: DDRAM[0x00-0x0F]
Row 1: DDRAM[0x40-0x4F]
```

### CGRAM (Character Generator RAM)

* Armazena ate 8 caracteres customizados
* 8 bytes por caractere

---

## Ciclo de escrita de dados

Uma escrita ocorre quando:

```
RS = 1
R/W = 0
E: HIGH -> LOW
```

### Comportamento do emulador

* Na borda de descida de `E`, captura os dados
* Escreve os dados em DDRAM ou CGRAM dependendo do modo de endereco
* Auto-incrementa ou decrementa o endereco conforme o entry mode

---

## Ciclo de escrita de instrucao

Uma escrita de comando ocorre quando:

```
RS = 0
R/W = 0
E: HIGH -> LOW
```

---

## Ciclo de leitura (opcional)

Leituras sao incomuns em sistemas hobby.

```
RS = 0/1
R/W = 1
E: HIGH
```

O emulador pode simplificar:

* Ignorar leituras por completo
* Ou retornar busy flag + address counter

---

## Emulacao do busy flag

### Hardware real

* Busy flag = D7
* Comandos levam 37-1520 us

### Opcoes de emulacao

| Modo       | Comportamento |
| ---------- | ------------- |
| Simplified | Always ready  |
| Timed      | Busy por N ciclos |

Padrao recomendado: **Always ready**

---

## Estado de power-up

No reset:

* Display OFF
* Cursor OFF
* DDRAM limpa ou indefinida
* Address counter = 0

O emulador deve:

* Limpar DDRAM
* Definir cursor em (0,0)
* Display habilitado

---

## Modelo de cursor e display

Variaveis de estado:

```text
cursor_row
cursor_col
display_on
cursor_on
blink_on
```

O cursor se move automaticamente apos escritas conforme o entry mode.

---

## Interface 4-bit vs 8-bit

### Modo 8-bit

* Byte completo transferido em D0-D7

### Modo 4-bit

* High nibble enviado primeiro
* Dois pulsos de enable por byte

Simplificacao do emulador:

* Aceitar escritas de byte completo
* Ignorar timing de nibble

---

## Modelo de renderizacao (UI do emulador)

Abordagem recomendada:

* Manter buffer de caracteres 16x2
* Renderizar subconjunto ASCII
* Substituir glifos nao suportados
* Opcionalmente renderizar caracteres CGRAM customizados

---

## Modelo de API do emulador

```c
typedef struct {
    uint8_t ddram[80];
    uint8_t cgram[64];
    uint8_t addr;
    bool display_on;
    bool cursor_on;
    bool blink_on;
    uint8_t entry_mode;
} FIT0127_LCD;
```

---

## Cabos comuns em sistemas 6502

```
VIA Port -> LCD D4-D7 (4-bit mode)
RS -> VIA bit
E  -> VIA bit
R/W -> GND
```

---

## Checklist de testes

* Clear display command
* Cursor positioning via DDRAM addresses
* Escritas sequenciais de caracteres
* Comportamento de line wrap
* Exibicao de caracteres customizados

---

## Referencias

* [HD44780U Datasheet (Hitachi)](https://academy.cba.mit.edu/classes/output_devices/44780.pdf)
* [Ben Eater LCD Interface Notes](https://hackaday.io/project/174128-db6502/log/181838-adventures-with-hd44780-lcd-controller)
* [Ben Eater's 6502 Computer](https://github.com/tedkotz/be6502)
* [Build a 6502 Computer](https://eater.net/6502)

---

## Notas

Esta especificacao prioriza intencionalmente o **comportamento visivel ao firmware** em vez da precisao eletrica, tornando-a ideal para:

* Emuladores de SBC
* Desenvolvimento de ROM e monitor
* Testes automatizados de saida de LCD
* Projetos educacionais de CPU
