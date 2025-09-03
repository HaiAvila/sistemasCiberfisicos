---
title: "Descripción de I2C"
parent: "Protocolo I2C"
nav_order: 1
layout: default
has_toc: false
---

# Descripción de I2C

**I2C** es un bus serie **síncrono** de dos hilos: **SDA** (datos) y **SCL** (reloj). Usa **direcciones de 7 bits** (común) o **10 bits**, con un **maestro** que inicia las transacciones y uno o varios **esclavos** que responden. Las líneas son **open-drain/open-collector**, por lo que requieren **resistencias pull-up**.

**Características clave (prácticas para la medición):**  
- **Topología:** maestro-esclavo; el maestro arbitra el bus e inicia `START`, dirección + R/W y `STOP`.  
- **Ack/Nack:** cada byte enviado recibe **ACK** si el receptor lo acepta.  
- **Frecuencias típicas:** `100 kHz` (Standard Mode), `400 kHz` (Fast Mode).  
- **Pull-ups:** típicamente **4.7 kΩ** hacia **3.3 V**; valores entre **2.2 kΩ y 10 kΩ** según la longitud/capacitancia del bus.  
- **Latencia:** depende del tiempo de servicio del esclavo, del **clock stretching** (si lo usa), de la frecuencia SCL y del tamaño de la trama.

**Pines habituales por plataforma (referencia):**  
- **Arduino Nano (ATmega328P):** `SDA = A4`, `SCL = A5` (**5 V**).  
- **ESP32 (Wire por defecto):** `SDA = GPIO21`, `SCL = GPIO22` (**3.3 V**, puede reconfigurarse).  
- **XIAO RP2040:** I2C0 típico en `SDA/SCL` de la placa (consultar pinmap de tu board, p. ej., D4/D5).  

**Formato de prueba sugerido:**  
- **Payload:** entero pequeño (ASCII `"0001"` o binario 2-bytes).  
- **Protocolo de eco:** el **maestro** escribe el valor; el **esclavo** calcula **valor+1** y lo devuelve en una lectura inmediata del mismo maestro (write→read).  
- **Medición:** el **maestro** toma `T₀` antes del `write` y `T₁` al terminar el `read` (dato recibido). Reportar **Δt**.

Con esto se garantiza comparabilidad con la sección de **UART** y se facilita la tabla de **Latencia y resultados (I2C)**.
