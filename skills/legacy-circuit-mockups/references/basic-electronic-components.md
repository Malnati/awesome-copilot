# Basic Electronic Components

Este documento cobre quatro componentes eletronicos fundamentais: resistors, capacitors, buttons e switches, incluindo valores comuns, formulas e tipos.

---

## 1. Resistors (R)

Resistors limitam o fluxo de corrente eletrica e dissipam energia como calor.

### Formulas Comuns

* **Ohm's Law:** $V = I \times R$ (Voltage = Current $\times$ Resistance)
* **Series Resistance:** $R_{total} = R_1 + R_2 + ... + R_n$
* **Parallel Resistance:** $\frac{1}{R_{total}} = \frac{1}{R_1} + \frac{1}{R_2} + ... + \frac{1}{R_n}$

### Common Values (E12/E24 Series - 5% Tolerance)

Os valores sao multiplicados por potencias de 10 (ex.: 10$\Omega$, 100$\Omega$, 1k$\Omega$, 10k$\Omega$, 100k$\Omega$, 1M$\Omega$):
**1.0, 1.1, 1.2, 1.3, 1.5, 1.6, 1.8, 2.0, 2.2, 2.4, 2.7, 3.0, 3.3, 3.6, 3.9, 4.3, 4.7, 5.1, 5.6, 6.2, 6.8, 7.5, 8.2, 9.1**

---

## 2. Capacitors (C)

Capacitors armazenam carga eletrica em um campo eletrico.

### Formulas Comuns

* **Capacitance:** $C = \frac{Q}{V}$ (Charge / Voltage)
* **Current:** $i = C \frac{dV}{dt}$
* **Parallel Capacitance:** $C_{total} = C_1 + C_2 + ... + C_n$
* **Series Capacitance:** $\frac{1}{C_{total}} = \frac{1}{C_1} + \frac{1}{C_2} + ... + \frac{1}{C_n}$
* **Energy Stored:** $W = \frac{1}{2} C V^2$

### Common Values

* **Ceramic (picoFarads/nanoFarads):** 10pF, 22pF, 100pF, 1nF, 10nF, 100nF (often labeled "104")
* **Electrolytic (microFarads):** 1µF, 10µF, 22µF, 47µF, 100µF, 220µF, 470µF, 1000µF

---

## 3. Buttons (Push Button)

Um button momentaneo que fecha um circuito apenas quando pressionado.

### Common Types

* **Normally Open (NO):** Circuit is open (off) until pushed.
* **Normally Closed (NC):** Circuit is closed (on) until pushed.
* **Tactile Switch:** Small, standard button for PCB mounting.

---

## 4. Switches (SW)

Um switch eletromecanico que abre ou fecha um circuito, mantendo a posicao ate ser acionado.

### Common Types

* **SPST:** Single Pole, Single Throw (On/Off)
* **SPDT:** Single Pole, Double Throw (Toggle between two paths)
* **DPST/DPDT:** Double Pole variants (controls two independent circuits simultaneously)
* **DIP Switch:** Small array of switches for circuit boards.

---

## Tabela Resumo de Referencia Rapida

| Component | Symbol (Ref) | Key Formula | Common Use |
| :--- | :--- | :--- | :--- |
| **Resistor** | Zigzag / Box | $V=IR$ | Current limiting, voltage divider |
| **Capacitor**| Two Parallel Lines| $i=C \frac{dV}{dt}$ | Filtering, timing, coupling |
| **Button** | Momentary | N/A | User input (momentary) |
| **Switch** | Toggle/Lever | N/A | Power control, signal routing |

---

## Common Suffixes

* **k** = kilo ($10^3$)
* **M** = Mega ($10^6$)
* **m** = milli ($10^{-3}$)
* **µ** = micro ($10^{-6}$)
* **n** = nano ($10^{-9}$)
* **p** = pico ($10^{-12}$)