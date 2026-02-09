# T48 Universal EEPROM / Flash Programmer Specification

## 1. Visao geral

O **T48 Universal Programmer** (tambem vendido como **TL866-3G / TL866II Plus successor**) e um dispositivo USB usado para **ler, programar e verificar** uma ampla gama de **EEPROM, Flash, EPROM, GAL e microcontrollers**. E comumente usado em workflows de **retrocomputing**, **desenvolvimento de firmware** e **reparo de eletronicos**.

O T48 e um substituto moderno para a serie TL866, oferecendo suporte expandido a dispositivos e melhor compatibilidade de software.

---

## 2. Caracteristicas gerais

| Recurso             | Descricao                              |
| ------------------- | -------------------------------------- |
| Tipo de programador | Universal device programmer            |
| Interface           | USB 2.0                                |
| Socket              | ZIF-48                                 |
| Dispositivos suportados | EEPROM, Flash, EPROM, GAL, MCU     |
| Tensao de programacao | Gerada internamente                  |
| Host OS             | Windows, Linux (open-source tools)     |
| Verificacao         | Read-after-write, checksum             |

---

## 3. Descricao fisica

### 3.1 ZIF Socket

* Socket **ZIF de 48 pinos (Zero Insertion Force)**
* Suporta pacotes DIP diretamente
* PLCC, SOP, TSOP suportados via adaptadores

### 3.2 Indicadores e controles

| Item          | Descricao                      |
| ------------- | ------------------------------ |
| Status LED    | Indicacao de energia / atividade |
| ZIF lever     | Trava o IC no socket           |
| USB connector | Conexao e energia do host      |

---

## 4. Categorias de dispositivos suportados

### 4.1 Dispositivos de memoria

* 27xxx EPROM
* 28xxx EEPROM (ex.: AT28C256)
* 29xxx Flash memory
* Serial EEPROMs (com adaptadores)

### 4.2 Logica programavel

* GAL16V8, GAL22V10 (read/write/verify)

### 4.3 Microcontrollers (limitado)

* Alguns dispositivos PIC, AVR e familia 8051
* Suporte de programacao depende da tensao e pinout

---

## 5. Capacidades eletricas

| Parametro     | Descricao                              |
| ------------- | -------------------------------------- |
| Faixa VCC     | 1.8 V - 6.5 V (dependente do dispositivo) |
| VPP           | Gerada internamente, device-specific    |
| Protecao I/O  | Protegido contra sobrecorrente e curto |

---

## 6. Operacoes de programacao

### 6.1 Operacoes comuns

* Identificacao de dispositivo
* Blank check
* Read
* Program (write)
* Verify
* Erase (Flash/EPROM)

### 6.2 Modos de verificacao

* Comparacao byte-a-byte
* Checksum / CRC validation

---

## 7. Suporte de software

### 7.1 Software oficial

* GUI baseada em Windows
* Database de dispositivos com pin mappings
* Controle automatico de tensao e timing

### 7.2 Ferramentas open-source

| Tool      | Notas                            |
| --------- | -------------------------------- |
| `minipro` | Linux / macOS / Windows CLI tool |
| libusb    | USB communication backend        |

Exemplo:

```bash
minipro -p AT28C256 -w rom.bin
```

---

## 8. Insercao de dispositivo e pin mapping

* Orientacao do dispositivo indicada por **pin 1 marker**
* Muitos dispositivos usam **alinhamento inferior esquerdo** no ZIF socket
* O software exibe o diagrama correto de insercao

---

## 9. Workflow tipico (exemplo EEPROM)

1. Selecionar tipo de dispositivo (ex.: AT28C256)
2. Inserir o chip no ZIF socket
3. Executar blank check
4. Carregar imagem binaria
5. Programar o dispositivo
6. Verificar conteudo escrito

---

## 10. Notas de energia e seguranca

* Programador alimentado totalmente via USB
* Nao insira ou remova ICs durante a programacao
* Use adaptadores para pacotes nao-DIP

---

## 11. Limitacoes

* Nem todos os microcontrollers sao suportados
* EPROMs de alta tensao podem exigir adaptadores especificos
* Nao destinado a in-circuit programming (ISP)

---

## 12. Casos de uso comuns

* Programar EEPROMs para 6502 SBCs
* Flashing de imagens ROM para sistemas retro
* Ler e fazer backup de EPROMs legadas
* Desenvolvimento de logica GAL

---

## 13. Comparacao com TL866II Plus

| Recurso            | TL866II Plus | T48             |
| ------------------ | ------------ | --------------- |
| Suporte a dispositivos | Good     | Expanded        |
| Suporte de OS       | Windows      | Windows / Linux |
| Ferramentas open-source | Limited | Excellent       |

---

## 14. Referencias

* <https://www.bulcomp-eng.com/datasheet/XGecu%20T48%20-%20Introduction.pdf>
* <https://gitlab.com/DavidGriffith/minipro>
* <https://opensource.com/article/23/1/learn-machine-language-retro-computer>

---
