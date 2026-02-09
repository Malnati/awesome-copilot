# AS6C62256 (32K x 8 SRAM) Emulation Specification

Uma especificacao tecnica em Markdown para **emular a AS6C62256 / familia 62256 de SRAM estatica**, adequada para emuladores da familia 6502, simuladores de SBC e modelagem de subsistemas de memoria.

---

## 1. Scope

Este documento especifica o comportamento funcional para emular:

* Alliance Memory **AS6C62256**
* Dispositivos **62256 (32K x 8) SRAM** compativeis

Fora de escopo:

* Margens de temporizacao eletrica analogica
* Contencao de barramento e tempos de subida/descida de sinal
* Caracteristicas de consumo de energia

---

## 2. Visao Geral do Chip

### Core Characteristics

| Feature        | Value                |
| -------------- | -------------------- |
| Memory Type    | Static RAM (SRAM)    |
| Capacity       | 32,768 bytes (32 KB) |
| Data Width     | 8-bit                |
| Address Width  | 15-bit (A0-A14)      |
| Access Type    | Asynchronous         |
| Supply Voltage | 5 V (typical)        |

---

## 3. External Signals (Logical Model)

| Signal | Direction | Purpose                    |
| ------ | --------- | -------------------------- |
| A0-A14 | Input     | Address bus                |
| D0-D7  | I/O       | Data bus                   |
| CE#    | Input     | Chip enable (active low)   |
| OE#    | Input     | Output enable (active low) |
| WE#    | Input     | Write enable (active low)  |

> `#` indica sinais active-low.

---

## 4. Address Space Mapping

* Address range: `0x0000-0x7FFF`
* Address lines select one byte per address

### Exemplo Tipico de Mapeamento de Sistema 6502

| CPU Address Range | Device         |
| ----------------- | -------------- |
| `$0000-$7FFF`     | AS6C62256 SRAM |
| `$8000-$FFFF`     | ROM / EEPROM   |

---

## 5. Read and Write Behavior

### Read Cycle (Logical)

Conditions:

* `CE# = 0`
* `OE# = 0`
* `WE# = 1`

Behavior:

```text
D[7:0] <- memory[A]
```

If `OE# = 1` or `CE# = 1`, data bus is **high-impedance**.

---

### Write Cycle (Logical)

Conditions:

* `CE# = 0`
* `WE# = 0`

Behavior:

```text
memory[A] <- D[7:0]
```

* `OE#` is ignored during writes
* Write occurs on active WE#

---

## 6. Control Signal Priority

| CE# | WE# | OE# | Result          |
| --- | --- | --- | --------------- |
| 1   | X   | X   | Desabilitado (Hi-Z) |
| 0   | 0   | X   | Write           |
| 0   | 1   | 0   | Read            |
| 0   | 1   | 1   | Hi-Z            |

---

## 7. Requisitos da Interface do Emulador

Um emulador deve expor:

```text
read(address)  -> byte
write(address, byte)
```

Armazenamento interno:

```text
uint8_t ram[32768]
```

Address masking:

```text
address = address & 0x7FFF
```

---

## 8. Timing Model (Abstracted)

### Emulation Levels

| Level          | Description               |
| -------------- | ------------------------- |
| Functional     | Instantaneous access      |
| Cycle-based    | Access per CPU cycle      |
| Cycle-accurate | Honors enable transitions |

Para a maioria dos sistemas, **functional emulation** e suficiente.

---

## 9. Power and Data Retention

* Conteudo da SRAM persiste enquanto houver alimentacao
* Emulador deve manter conteudo ate reset explicito

### Reset Behavior

* **No automatic clearing** on reset
* Conteudo indefinido se nao for inicializado

---

## 10. Bus Contention and Hi-Z Modeling (Optional)

Comportamento avancado opcional:

* Rastrear quando a SRAM dirige o data bus
* Detectar escritas simultaneas ilegais

A maioria dos emuladores pode ignorar estado Hi-Z.

---

## 11. Condicoes de Erro

| Condition            | Emulator Response             |
| -------------------- | ----------------------------- |
| Out-of-range address | Mask or ignore                |
| Read when disabled   | Return last bus value or 0xFF |
| Write when disabled  | Ignore write                  |

---

## 12. Integration with 6502 Emulator

```text
CPU memory access
  -> address decode
  -> if in SRAM range:
      AS6C62256.read/write
```

* Acesso a SRAM geralmente e de ciclo unico
* Nao ha wait states necessarios

---

## 13. Testing and Validation

### Basic Tests

* Write/read patterns
* Boundary addresses ($0000, $7FFF)
* Randomized memory tests

### Validation Checklist

* Writes persist
* Reads return correct values
* Address wrapping correct

---

## 14. Common Mistakes

| Mistake               | Result                      |
| --------------------- | --------------------------- |
| Clearing RAM on reset | Breaks software assumptions |
| Wrong address mask    | Mirrored memory errors      |
| Treating as ROM       | Writes ignored              |

---

## 15. Reference Links

* [Alliance Memory AS6C62256 Datasheet](https://www.alliancememory.com/wp-content/uploads/AS6C62256-23-March-2016-rev1.2.pdf)
* [https://en.wikipedia.org/wiki/Static_random-access_memory](https://en.wikipedia.org/wiki/Static_random-access_memory)

---

**Document Scope:** Software emulation of AS6C62256 SRAM
**Audience:** Emulator developers, retro SBC designers
**Status:** Stable technical reference