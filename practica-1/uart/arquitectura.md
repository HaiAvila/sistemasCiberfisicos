---
title: "Arquitectura y conexiones"
parent: "UART"
nav_order: 1
layout: default
---

# Arquitectura y descripción de UART

**UART** (Universal Asynchronous Receiver/Transmitter) es un método de comunicación serie **asíncrono**, que no requiere reloj compartido: cada trama usa bits de **inicio** y **parada** para sincronizar.

Características clave:
- **Punto a punto:** dos líneas de datos (TX y RX) más GND.  
- **Full-duplex:** transmisión y recepción simultáneas.  
- **Formato común:** 8N1 (8 bits, sin paridad, 1 bit de parada).  
- **Baud rate típico:** 9 600 a varios Mbaudios (según hardware).  

En esta práctica se fijó inicialmente **38400 baud** para comparar la latencia entre **Arduino Nano (ATmega328P)**, **XIAO RP2040** y **ESP32** en condiciones homogéneas.

---

## Pines y conexiones

- **TX ↔ RX cruzados**.  
- **GND común** entre dispositivos.  
- Precaución: **Arduino Nano es 5V**, mientras que RP2040 y ESP32 son **3.3V** → usar divisor o level shifter si es necesario.  

**Esquema en KiCad:**  
![Esquema UART](/assets/img/uart/esquemas/uart_diagrama.png)

**Foto de conexión real:**  
![Foto conexión](/assets/img/uart/conexiones/uart_rp2040_esp32.jpg)
