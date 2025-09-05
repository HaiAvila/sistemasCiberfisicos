---
title: "Firmware"
parent: "Ejercicio 4 — Punto a Multipunto"
layout: default
nav_order: 2
---

# Firmware — roles y mensajes

**Convenciones**
- Formato de mensaje: entero de 4 dígitos + salto de línea (`"%04d\n"`).  
- Device A = **Maestro**.  
- Devices B y C = **Esclavos**.  

**Códigos por plataforma**
- `/codigos/nano/` → ATmega328P.  
- `/codigos/rp2040/` → Raspberry Pi RP2040.  
- `/codigos/esp32/` → ESP32.  

Cada carpeta tendrá dos archivos:  
- `*-maestro.*`  
- `*-esclavo.*`  
