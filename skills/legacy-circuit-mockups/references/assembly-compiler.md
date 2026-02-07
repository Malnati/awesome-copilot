# 6502 SBC Assembly Compilation & ROM Build Specification

## Visao geral

Este documento define uma **especificacao completa para compilar assembly 6502** para um single-board computer composto por:

* **MOS 6502 CPU**
* **MOS 6522 VIA**
* **AS6C62256 (32 KB SRAM)**
* **AT28C256 (32 KB EEPROM / ROM)**
* **DFRobot FIT0127 (HD44780-compatible 16x2 LCD)**

O foco e **comportamento da toolchain, layout de memoria, construcao de ROM e convencoes de firmware**, nao a fiação eletrica.

---

## Arquitetura do sistema alvo

### Mapa de memoria (canonico)

```
$0000-$00FF  Zero Page (RAM)
$0100-$01FF  Stack (RAM)
$0200-$7FFF  General RAM (AS6C62256)
$8000-$8FFF  6522 VIA I/O space
$9000-$FFFF  ROM (AT28C256)
```

> Address decoding pode espelhar dispositivos; o assembler assume este layout canonico.

---

## Organizacao de ROM (AT28C256)

| Endereco   | Proposito           |
| ---------- | ------------------- |
| $9000-$FFEF | Program code + data |
| $FFF0-$FFF9 | Optional system data |
| $FFFA-$FFFB | NMI vector          |
| $FFFC-$FFFD | RESET vector        |
| $FFFE-$FFFF | IRQ/BRK vector      |

Tamanho da imagem ROM: **32,768 bytes**

---

## Convencao de reset e startup

No reset:

1. CPU busca o RESET vector em `$FFFC`
2. Codigo inicializa o stack pointer
3. Variaveis de zero-page inicializadas
4. VIA configurada
5. LCD inicializado
6. Programa principal iniciado

---

## Requisitos do assembler

O assembler **DEVE** suportar:

* `.org` absolute addressing
* Labels simbolicos
* Saida binaria (`.bin`)
* Emissao de word little-endian
* Zero-page optimization

Assemblers recomendados:

* **ca65** (cc65 toolchain)
* **vasm6502**
* **64tass**

---

## Estrutura do source assembly

```asm
;---------------------------
; Reset Vector Entry Point
;---------------------------
        .org $9000
RESET:
        sei
        cld
        ldx #$FF
        txs
        jsr init_via
        jsr init_lcd
MAIN:
        jsr lcd_print
        jmp MAIN
```

---

## Definicao da tabela de vetores

```asm
        .org $FFFA
        .word nmi_handler
        .word RESET
        .word irq_handler
```

---

## Modelo de programacao do 6522 VIA

### Mapa de registradores (Base = $8000)

| Offset | Register |
| ------ | -------- |
| $0     | ORB      |
| $1     | ORA      |
| $2     | DDRB     |
| $3     | DDRA     |
| $4     | T1CL     |
| $5     | T1CH     |
| $6     | T1LL     |
| $7     | T1LH     |
| $8     | T2CL     |
| $9     | T2CH     |
| $B     | ACR      |
| $C     | PCR      |
| $D     | IFR      |
| $E     | IER      |

---

## Convencao de interface LCD

### Assuncao de fiação do LCD

| LCD   | VIA     |
| ----- | ------- |
| D4-D7 | PB4-PB7 |
| RS    | PA0     |
| E     | PA1     |
| R/W   | GND     |

Modo 4-bit assumido.

---

## Sequencia de inicializacao do LCD

```asm
lcd_init:
        lda #$33
        jsr lcd_cmd
        lda #$32
        jsr lcd_cmd
        lda #$28
        jsr lcd_cmd
        lda #$0C
        jsr lcd_cmd
        lda #$06
        jsr lcd_cmd
        lda #$01
        jsr lcd_cmd
        rts
```

---

## Interface de comando/dados do LCD

| Operacao | RS | Data            |
| -------- | -- | --------------- |
| Command  | 0  | Instruction     |
| Data     | 1  | ASCII character |

---

## Convencao de uso de zero page

| Endereco | Proposito   |
| -------- | ----------- |
| $00-$0F  | Scratch     |
| $10-$1F  | LCD routines |
| $20-$2F  | VIA state   |
| $30-$FF  | User-defined |

---

## Uso de RAM (AS6C62256)

* Stack usa a page `$01`
* Toda RAM e assumida volatil
* Sem ROM shadowing

---

## Pipeline de build

### Step 1: Assemble

```bash
ca65 main.asm -o main.o
```

### Step 2: Link

```bash
ld65 -C rom.cfg main.o -o rom.bin
```

### Step 3: Pad ROM

Garanta que `rom.bin` tenha exatamente **32768 bytes**.

---

## Programacao de EEPROM

* Dispositivo alvo: **AT28C256**
* Programador: **MiniPro / T48**
* Verificar apos escrever

---

## Expectativas do emulador

O emulador deve:

* Carregar ROM em `$9000-$FFFF`
* Emular efeitos colaterais de I/O da VIA
* Renderizar saida do LCD
* Respeitar RESET vector

---

## Checklist de testes

* Execucao do RESET vector
* Escritas nos registradores VIA
* LCD exibe texto correto
* Operacoes de stack validas
* Imagem ROM mapeia corretamente

---

## Referencias

* [MOS 6502 Programming Manual](http://archive.6502.org/datasheets/synertek_programming_manual.pdf)
* [MOS 6522 VIA Datasheet](http://archive.6502.org/datasheets/mos_6522_preliminary_nov_1977.pdf)
* [AT28C256 Datasheet](https://ww1.microchip.com/downloads/aemDocuments/documents/MPD/ProductDocuments/DataSheets/AT28C256-Industrial-Grade-256-Kbit-Paged-Parallel-EEPROM-Data-Sheet-DS20006386.pdf)
* [HD44780 LCD Datasheet](https://www.futurlec.com/LED/LCD16X2BLa.shtml)
* [cc65 Toolchain Docs](https://cc65.github.io/doc/cc65.html)

---

## Notas

Esta especificacao e intencionalmente **end-to-end**: do source assembly ate a imagem EEPROM e a execucao em hardware ou emulador. Ela define um contrato estavel para que ROMs, emuladores e SBCs reais se comportem de forma identica.
