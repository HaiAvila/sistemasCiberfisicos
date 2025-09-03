---
title: "02 — Metodología de medición"
parent: "Práctica 1"
nav_order: 4
layout: default
has_toc: true
---

# Metodología

## Qué medimos

- **Latencia round-trip (RTT)**: tiempo desde que el **Maestro** envía un payload hasta que recibe el **eco** del **Esclavo**.  
- Ejecuta **1,000 mensajes** por combinación de placas y parámetros.

## Procedimiento general

1. Conecta según **Plataformas** (tabla de pines) y verifica **GND común**.  
2. Carga `Maestro` en placa A y `Esclavo` en placa B.  
3. Abre monitor/cliente que **registre CSV** con el formato anterior.  
4. Envía **1,000 mensajes** con el payload definido (p. ej. 16 bytes).  
5. Repite para los otros parámetros y cambia roles A/B.  
6. Copia los CSV a `assets/logs/<proto>/`.

## Gráficas y resumen

Para cada protocolo genera:

- **Histograma** de `rtt_ms`  
- **Serie temporal** (rtt vs índice)  
- **Boxplot**  
- **Tabla** con `mean`, `median`, `p95`, `std` y **jitter = p95 − median**.

*(Puedes graficar en Python/Excel; en la sección de cada protocolo colocaremos las 6 gráficas requeridas + interpretación.)*

## Troubleshooting rápido

- Nada responde: revisa **pines invertidos** (TX↔RX), **pull-ups I2C**, **CS** correcto en SPI.  
- Eco corrupto: baja velocidad o sincroniza baud/clocks.  
- Nano con 5 V: protege entradas 3.3 V (RP2040/ESP32).