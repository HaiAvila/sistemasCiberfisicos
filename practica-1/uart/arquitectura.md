---
title: "Arquitectura y conexiones"
parent: "UART"
nav_order: 1
layout: default
---

# Arquitectura y conexiones (UART)

## Topologías a medir
- RP2040 ↔ ESP32
- RP2040 ↔ Arduino Nano
- ESP32 ↔ Arduino Nano

> **Reglas básicas:** TX↔RX cruzados y **GND común**.

## Pinout propuesto
*(ajusta si tus placas difieren)*

- **RP2040 (XIAO):** TX=GP0, RX=GP1  
- **ESP32:** TX=GPIO1 (o 17), RX=GPIO3 (o 16)  
- **Nano:** TX=D1, RX=D0 (**comparterna USB-serial**)

## Esquema y fotos
- **Esquema KiCad:** *inserta PNG/SVG exportado*  
  ![](/assets/img/uart/esquemas/uart_rp2040_esp32.png)
- **Conexión real:**  
  ![](/assets/img/uart/conexiones/uart_rp2040_esp32_real.jpg)
