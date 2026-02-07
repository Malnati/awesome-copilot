# 6502 Assembly Language with AT28C256 EEPROM

Uma especificacao pratica para escrever programas em **assembly 6502/65C02** destinados a serem armazenados e executados em uma **AT28C256 (32 KB) EEPROM paralela** em single-board computers (SBCs) e sistemas retro.

---

## 1. Scope and Assumptions

Este documento assume:

* Uma **CPU da familia 6502** (6502, 65C02 ou compatível)
* Codigo do programa armazenado em uma **AT28C256 (32K x 8) EEPROM**
* I/O mapeado em memoria (ex.: 6522 VIA)
* Vetores de reset e interrupcao localizados na EEPROM
* RAM externa mapeada em outro endereco (ex.: 62256 SRAM)

---

## 2. Visao Geral da EEPROM AT28C256

| Parameter      | Value               |
| -------------- | ------------------- |
| Capacity       | 32 KB (32768 bytes) |
| Address Lines  | A0-A14              |
| Data Lines     | D0-D7               |
| Access Time    | ~150 ns             |
| Supply Voltage | 5 V                 |
| Package        | DIP-28 / PLCC       |

### Typical Memory Map Usage

| Address Range | Usage                   |
| ------------- | ----------------------- |
| `$8000-$FFFF` | EEPROM (code + vectors) |
| `$FFFA-$FFFF` | Interrupt vectors       |

---

## 3. Exemplo de Mapa de Memoria 6502

```
$0000-$00FF  Zero Page (RAM)
$0100-$01FF  Stack
$0200-$7FFF  RAM / I/O
$8000-$FFFF  AT28C256 EEPROM
```

---

## 4. Reset and Interrupt Vectors

O 6502 le vetores no **topo da memoria**:

| Vector  | Address       | Description            |
| ------- | ------------- | ---------------------- |
| NMI     | `$FFFA-$FFFB` | Non-maskable interrupt |
| RESET   | `$FFFC-$FFFD` | Reset entry point      |
| IRQ/BRK | `$FFFE-$FFFF` | Maskable interrupt     |

### Exemplo de Definicao de Vetor

```asm
        .org $FFFA
        .word nmi_handler
        .word reset
        .word irq_handler
```

---

## 5. Assembly Program Structure

### Typical Layout

```asm
        .org $8000

reset:
        sei             ; Disable IRQs
        cld             ; Clear decimal mode
        ldx #$FF
        txs             ; Initialize stack

main:
        jmp main
```

---

## 6. Essential 6502 Instructions

### Registers

| Register | Purpose          |
| -------- | ---------------- |
| A        | Accumulator      |
| X, Y     | Index registers  |
| SP       | Stack pointer    |
| PC       | Program counter  |
| P        | Processor status |

### Common Instructions

| Instruction | Function               |
| ----------- | ---------------------- |
| LDA/STA     | Load/store accumulator |
| LDX/LDY     | Load index registers   |
| JMP/JSR     | Jump / subroutine      |
| RTS         | Return from subroutine |
| BEQ/BNE     | Conditional branch     |
| SEI/CLI     | Disable/enable IRQ     |

---

## 7. Addressing Modes (Common)

| Modo      | Exemplo       | Notas        |
| --------- | ------------- | ------------ |
| Immediate | `LDA #$01`    | Constant     |
| Zero Page | `LDA $00`     | Fast         |
| Absolute  | `LDA $8000`   | Full address |
| Indexed   | `LDA $2000,X` | Tables       |
| Indirect  | `JMP ($FFFC)` | Vectors      |

---

## 8. Writing Code for EEPROM Execution

### Key Considerations

* Codigo e **read-only em runtime**
* Self-modifying code nao e recomendado
* Coloque jump tables e constantes na EEPROM
* Use RAM para variaveis e stack

### Exemplo de Variavel de Zero Page

```asm
counter = $00

        lda #$00
        sta counter
```

---

## 9. Timing and Performance

* O tempo de acesso da EEPROM deve atender aos requisitos de clock da CPU
* AT28C256 suporta ~1 MHz confortavelmente
* Clocks mais rapidos podem exigir wait states ou ROM shadowing

---

## 10. Exemplo: Toggle de LED Simples (I/O com Memoria Mapeada)

```asm
PORTB = $6000
DDRB  = $6002

        .org $8000
reset:
        sei
        ldx #$FF
        txs

        lda #$FF
        sta DDRB

loop:
        lda #$FF
        sta PORTB
        jsr delay
        lda #$00
        sta PORTB
        jsr delay
        jmp loop
```

---

## 11. Assembling and Programming Workflow

1. Write source (`.asm`)
2. Assemble to binary
3. Pad or relocate to `$8000`
4. Program AT28C256 via T48 / minipro
5. Insert EEPROM and reset CPU

---

## 12. Assembler Directives (Common)

| Directive  | Purpose                     |
| ---------- | --------------------------- |
| `.org`     | Set program origin          |
| `.byte`    | Define byte                 |
| `.word`    | Define word (little-endian) |
| `.include` | Include file                |
| `.equ`     | Constant definition         |

---

## 13. Common Mistakes

| Issue                      | Result             |
| -------------------------- | ------------------ |
| Missing vectors            | CPU hangs on reset |
| Wrong `.org`               | Code not executed  |
| Using RAM addresses in ROM | Crash              |
| Stack not initialized      | Undefined behavior |

---

## 14. Reference Links

* [https://www.masswerk.at/6502/6502_instruction_set.html](https://www.masswerk.at/6502/6502_instruction_set.html)
* [https://www.nesdev.org/wiki/6502](https://www.nesdev.org/wiki/6502)
* [https://www.westerndesigncenter.com/wdc/documentation](https://www.westerndesigncenter.com/wdc/documentation)
* [https://en.wikipedia.org/wiki/MOS_Technology_6502](https://en.wikipedia.org/wiki/MOS_Technology_6502)

---

**Document Scope:** 6502 assembly stored in AT28C256 EEPROM
**Audience:** Retrocomputing, SBC designers, embedded hobbyists
**Status:** Stable reference