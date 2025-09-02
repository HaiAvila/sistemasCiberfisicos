---
layout: default
title: Descripción de UART
nav_order: 1
parent: "Protocolo UART"
has_toc: false
---

# Descripción de UART

**UART** (Universal Asynchronous Receiver/Transmitter) es un método de comunicación serie **asíncrono** que no requiere reloj compartido: cada trama usa bits de **inicio** y **parada** para sincronizar.  
Características clave:
- **Punto a punto**: dos líneas de datos (**TX** y **RX**) más **GND**.
- **Full-duplex**: transmisión y recepción simultáneas.
- **Formato común**: 8N1 (8 bits, sin paridad, 1 bit de parada).
- **Baud rate** típico: desde 9600 hasta varios Mbaudios (según hardware y calidad de enlace).

En esta práctica se fijó **38400 baud** para comparar la latencia entre **Arduino Nano (ATmega328P)**, **XIAO RP2040** y **ESP32** en condiciones homogéneas.
