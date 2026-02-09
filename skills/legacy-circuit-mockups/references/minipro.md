# minipro Chip Programming Utility Specification

## 1. Visao geral

O **minipro** e um utilitario open-source de linha de comando usado para **programar, ler, apagar e verificar** uma ampla gama de **EEPROM, Flash, EPROM, SRAM, GAL e dispositivos logicos** usando programadores universais suportados como **T48**, **TL866II Plus** e modelos compativeis.

E amplamente usado em ambientes **Linux**, **macOS** e **Windows**, especialmente para **retrocomputing**, **desenvolvimento de firmware** e **prototipagem eletronica**.

---

## 2. Programadores suportados

| Programador  | Notas                      |
| ----------- | -------------------------- |
| T48         | Suporte total (recomendado) |
| TL866II Plus | Suporte total             |
| TL866A / CS | Suporte limitado / legado  |

---

## 3. Tipos de dispositivos suportados

### 3.1 Dispositivos de memoria

* Parallel EEPROM (ex.: AT28C256)
* Flash memory (serie 29xxx)
* EPROM (serie 27xxx)
* SRAM (somente read/verify)

### 3.2 Logica e PLDs

* GAL16V8 / GAL22V10
* Dispositivos PAL (limitado)

### 3.3 Outros dispositivos

* Alguns microcontrollers (dependente do dispositivo)
* Teste de ICs logicos (modelos selecionados)

---

## 4. Instalacao

### 4.1 Linux

```bash
sudo apt install minipro
```

ou via source:

```bash
git clone https://github.com/vdudouyt/minipro.git
make
sudo make install
```

### 4.2 Windows

* Instalar via MSYS2 ou binarios precompilados
* Requer driver libusb (WinUSB)

---

## 5. Sintaxe basica de comando

```bash
minipro [options]
```

Options comuns:

| Option        | Descricao              |
| ------------- | ---------------------- |
| `-p <device>` | Seleciona dispositivo alvo |
| `-r <file>`   | Le dispositivo para arquivo |
| `-w <file>`   | Escreve arquivo no dispositivo |
| `-e`          | Apaga dispositivo       |
| `-v`          | Verifica conteudo       |
| `-I`          | Informacoes do dispositivo |
| `-l`          | Lista dispositivos suportados |

---

## 6. Operacoes comuns de programacao

### 6.1 Listar dispositivos suportados

```bash
minipro -l
```

### 6.2 Identificar dispositivo

```bash
minipro -p AT28C256 -I
```

### 6.3 Ler um chip

```bash
minipro -p AT28C256 -r rom_dump.bin
```

### 6.4 Escrever um chip

```bash
minipro -p AT28C256 -w rom.bin
```

### 6.5 Verificacao apenas

```bash
minipro -p AT28C256 -v rom.bin
```

---

## 7. Programando EEPROMs (exemplo AT28C256)

```bash
minipro -p AT28C256 -w monitor.bin
```

* Software Data Protection e tratado automaticamente
* Delays do ciclo de escrita sao gerenciados internamente
* Verificacao executada apos a programacao

---

## 8. Programando Flash memory

```bash
minipro -p SST39SF040 -e -w firmware.bin
```

* Etapa de erase exigida para dispositivos Flash
* Sector erase tratado automaticamente

---

## 9. Operacoes com EPROM

```bash
minipro -p 27C256 -r eprom.bin
```

* UV erase exigido antes de reprogramar
* minipro verifica estado blank antes de escrever

---

## 10. Programacao de GAL

```bash
minipro -p GAL22V10 -w logic.jed
```

* Usa arquivos JEDEC
* Suporta read, write e verify
* Fuse maps visiveis via `-I`

---

## 11. Tratamento de erros e mensagens

| Mensagem              | Significado                |
| --------------------- | -------------------------- |
| `Device not found`    | Selecionado dispositivo incorreto |
| `Verification failed` | Dados nao conferem          |
| `Chip protected`      | Write protection habilitada |
| `Overcurrent detected` | Erro de insercao ou fiação |

---

## 12. Seguranca e boas praticas

* Confirme sempre a orientacao do dispositivo no ZIF socket
* Use o identificador correto (`-p`)
* Nao insira chips durante operacao
* Use adaptadores para encapsulamentos PLCC, SOP, TSOP

---

## 13. Workflow tipico de retrocomputing

1. Montar imagem da ROM
2. Programar EEPROM usando minipro + T48
3. Verificar conteudo
4. Instalar chip no SBC
5. Testar boot do sistema

---

## 14. Limitacoes

* Nem todos os dispositivos sao suportados
* Alguns microcontrollers requerem ferramentas proprietarias
* In-circuit programming (ISP) nao suportado

---

## 15. Referencias

* <https://gitlab.com/DavidGriffith/minipro>
* <https://www.hadex.cz/spec/m545b.pdf>
* <https://github.com/mikeroyal/Firmware-Guide>
* <https://mike42.me/blog/2021-08-a-first-look-at-programmable-logic>
* <https://retrocomputingforum.com/>

---
