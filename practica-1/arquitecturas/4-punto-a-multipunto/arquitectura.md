---
title: "Arquitectura"
parent: "Arq.1 — Punto a Multipunto"
layout: default
nav_order: 1
---

# Arquitectura (Ejercicio 4)

**Descripción**  
- **Device A:** contiene dos switches de entrada (`Switch1`, `Switch2`).  
- **Device B:** controla `LED1`.  
- **Device C:** controla `LED2`.  
- El dispositivo maestro es **A**, que envía comandos a B y C.  

**Objetivo**
Implementar la arquitectura descrita con todas las plataformas (por ejemplo que por rondas, todos los dispositivos sean ESP32, NANO, RP2040) y al usar cada plataforma probar cada protocolo (UART, SPI e I2C).

**Diagrama de bloques**
![Conexión]({{ "/assets/img/arq1.png" | relative_url }})
