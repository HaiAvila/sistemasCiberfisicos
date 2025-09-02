---
title: "Códigos"
parent: "Protocolo UART"
nav_order: 3
layout: default
has_toc: true
---

# Códigos

Convenciones de la práctica:
- **Baud:** 38400.
- **Formato de trama:** entero de 4 dígitos + `\n` (ej. `0001\n`).
- **Roles:** *Maestro* inicia ping y mide RTT; *Esclavo* hace **eco+1** inmediato.
- **Pines y niveles:** TX↔RX cruzado, **GND común**. Nano (3.3V) vs ESP32/RP2040 (3.3 V)

## Índice rápido
- [RP2040 (XIAO) — Maestro, MicroPython](#rp2040-maestro-micropython)
- [Arduino Nano — Esclavo (echo+1), SoftwareSerial](#arduino-esclavo-softwareserial)
- [Arduino Nano — Maestro (RTT), SoftwareSerial](#arduino-maestro-softwareserial)
- [RP2040 (XIAO) — Esclavo, MicroPython (echo+1)](#rp2040-esclavo-micropython)
- [Nota: Nano 33 — Maestro (sketch que enviaste)](#nota-nano33)