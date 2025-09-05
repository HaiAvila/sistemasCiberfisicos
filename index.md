---
title: "Práctica 1"
nav_order: 1
layout: default
has_toc: false
---

# Práctica 1

Este sitio documenta la **Práctica 1 de Sistemas Ciberfísicos** y está pensado como **tutorial replicable**: cualquier persona puede repetir las pruebas, obtener los mismos archivos de log y reproducir las gráficas.

**¿Qué harás?**  

- Conectar **RP2040**, **ESP32** y **ATmega328 (Arduino Nano)** utilizando los protocolos de comunicación **UART, I2C y SPI**.  
- Identificar las **características de cada plataforma** (frecuencia, memoria, pines, protocolos disponibles).  
- Medir la **latencia (round-trip)** enviando **1,000 mensajes** en cada combinación de dos dispositivos, primero con **UART**, luego con **I2C**, y comparar los resultados.  
- Generar **6 gráficas de latencia** para cada protocolo (UART e I2C) con las distintas combinaciones de plataformas.  
- Implementar arquitecturas mixtas con los **tres protocolos (UART, I2C y SPI)**, incluyendo un escenario jerárquico: **RP2040 ↔ ESP32 (I2C)** y **ESP32 ↔ ATmega328 (UART)**.  
- Documentar todo con **diagramas de arquitectura y esquemas eléctricos en KiCad**, además de los **códigos implementados**.  
- Elaborar una **tabla resumen** con métricas clave (promedio, mediana, p95, desviación estándar, jitter) y un análisis comparativo **UART vs I2C vs SPI**.


---

## Cómo replicar en 5 pasos

1. Lee **[Requisitos]({{ "/practica-1/00-requisitos/" | relative_url }})** (BoM, instalaciones y checklist).
2. Revisa **[Plataformas]({{ "/practica-1/01-plataformas/" | relative_url }})** (fichas y pines).
3. Sigue la **[Metodología]({{ "/practica-1/02-metodologia/" | relative_url }})** (formato de logs, nomenclatura).
4. Entra al protocolo que te interese (en construcción):
   - UART · I2C · SPI (conexiones, firmware, pruebas, resultados).
5. Compara y concluye.

> ¿Equipo autor? Sebastián Méndez, Haili Ávila y Daniela Colín.

---

## Índice de la práctica

1. [Requisitos]({{ "/practica-1/Requisitos.md" | relative_url }})
2. [Plataformas]({{ "/practica-1/Plataformas.md" | relative_url }})
3. [Metodología]({{ "/practica-1/Metodologia.md" | relative_url }})
