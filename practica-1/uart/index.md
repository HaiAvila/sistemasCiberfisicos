---
title: "UART"
nav_order: 10
layout: default
has_children: true
---

# UART — Objetivo y entregables

**Objetivo:** medir **baudrate máximo** por plataforma y **latencia round-trip** entre pares de placas (RP2040, ESP32, Nano) enviando **1000 mensajes** por corrida.  

**Entregables de esta sección:**
- Diagrama de **arquitectura** y **esquema eléctrico (KiCad)**.
- **Códigos** (maestro/esclavo) por plataforma.
- **6 gráficas** de latencia (todas las combinaciones de plataformas).
- **Tabla resumen** con mean, median, p95, std y jitter.

**Subsecciones:**
1. [Arquitectura y conexiones]({{ "/practica-1/uart/arquitectura/" | relative_url }})
2. [Firmware]({{ "/practica-1/uart/firmware/" | relative_url }})
3. [Pruebas]({{ "/practica-1/uart/pruebas/" | relative_url }})
4. [Resultados]({{ "/practica-1/uart/resultados/" | relative_url }})
