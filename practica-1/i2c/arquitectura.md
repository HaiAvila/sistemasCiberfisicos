---
title: "Arquitectura y conexiones"
parent: "I2C"
nav_order: 1
layout: default
---

# Arquitectura y descripción de I2C

**I2C** (Inter-Integrated Circuit) es un método de comunicación serie **síncrono**, que requiere una línea de datos (**SDA**) y una de reloj (**SCL**) compartidas.

Características clave:
- **Bus multipunto:** un maestro puede comunicar con múltiples esclavos.  
- **Señales compartidas:** SDA (datos) y SCL (reloj) + GND común.  
- **Velocidades típicas:** 100 kbps, 400 kbps, 1 Mbps o más.  
- **Direcciones únicas:** cada dispositivo tiene una dirección de 7 o 10 bits.  

En esta práctica se fijó inicialmente el modo **100 kHz** para comparar la latencia entre **Arduino Nano (ATmega328P)**, **XIAO RP2040** y **ESP32** en condiciones homogéneas.

---

## Pines y conexiones

- **SDA y SCL compartidos** en el bus.  
- **Pull-ups obligatorios** en SDA y SCL.  
- **GND común** entre dispositivos.  

**Esquema en KiCad:**  
![Esquema I2C](/assets/img/i2c/esquemas/i2c_diagrama.png)

**Foto de conexión real:**  
![Foto conexión](/assets/img/i2c/conexiones/i2c_rp2040_esp32.jpg)
