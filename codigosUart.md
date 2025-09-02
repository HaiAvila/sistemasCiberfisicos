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
- **Pines y niveles:** TX↔RX cruzado, **GND común**.
