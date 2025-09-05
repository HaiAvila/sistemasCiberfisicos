---
title: "Requisitos"
nav_order: 2
layout: default
has_toc: true
---

# Requisitos

## Material (BoM)

| Componente | Cant. | Notas |
|---|---:|---|
| RP2040 (ej. RP2040 Zero) | 3 | 3.3V lógico |
| ESP32 DevKit | 3 | 3.3V lógico |
| Arduino Nano (ATmega328P) | 3 | 5V lógico |
| Protoboard + jumpers | 5 |  |
| Resistencias **pull-up** (I2C 4.7k–10k) | 2 | SDA y SCL |
| Cables USB (según placas) | 6 | Para flasheo y energía |
| LEDs + resistencias 330Ω | 3 | Para pruebas maestro/esclavo |

> **Tip:** siempre comparte **GND común** entre todas las placas.

## Software

- **IDE/Toolchains:** Arduino IDE.
- **Drivers** para cada placa instalados.
- **Python 3.x** (si usarás script para graficar) y librerías estándar.



## Checklist de réplica

- [ ] Puedo compilar y subir firmware a **RP2040**, **ESP32** y **Nano**.  
- [ ] Veo cada placa en el **Monitor Serie**.  
- [ ] Tengo **pull-ups** en I2C y verifiqué **GND común**.  
- [ ] Puedo guardar CSVs.  
- [ ] Entiendo el **formato de CSV** y la **nomenclatura** (ver Metodología).
