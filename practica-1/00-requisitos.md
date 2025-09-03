---
title: "00 — Requisitos"
parent: "Práctica 1"
nav_order: 2
layout: default
has_toc: true
---

# Requisitos

## Material (BoM)

| Componente | Cant. | Notas |
|---|---:|---|
| RP2040 (ej. RP2040 Zero) | 1 | 3.3V lógico |
| ESP32 DevKit | 1 | 3.3V lógico |
| Arduino Nano (ATmega328P) | 1 | 5V lógico |
| Protoboard + jumpers | 1 |  |
| Resistencias **pull-up** (I2C 4.7k–10k) | 2 | SDA y SCL |
| Cables USB (según placas) | 3 | Para flasheo y energía |
| LEDs + resistencias 220Ω | 2–4 | Para pruebas maestro/esclavo |
| (Opcional) Nivelador lógico | 1 | Si mezclas 5V con 3.3V |

> **Tip:** siempre comparte **GND común** entre todas las placas.

## Software

- **IDE/Toolchains:** Arduino IDE 2.x / PlatformIO (a tu elección).
- **Drivers** para cada placa instalados.
- **Python 3.x** (si usarás script para graficar) y librerías estándar.



## Checklist de réplica

- [ ] Puedo compilar y subir firmware a **RP2040**, **ESP32** y **Nano**.  
- [ ] Veo cada placa en el **Monitor Serie**.  
- [ ] Tengo **pull-ups** en I2C y verifiqué **GND común**.  
- [ ] Puedo guardar CSVs en `assets/logs/<protocolo>/`.  
- [ ] Entiendo el **formato de CSV** y la **nomenclatura** (ver Metodología).
