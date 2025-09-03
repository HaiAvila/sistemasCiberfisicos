---
title: "Pruebas"
parent: "UART"
grand_parent: "Práctica 1"
nav_order: 3
layout: default
---

# Procedimiento de pruebas (UART)

## Parte A — Descubrir baudrate máximo por plataforma
1. Conecta un **eco local** (misma placa) o par simple estable.
2. Incrementa baud: 115200 → 230400 → 460800 → 921600 → 1M (si aplica).
3. Criterio: **0 pérdidas** y caracteres íntegros durante 30–60 s.

## Parte B — Latencia entre plataformas (1000 mensajes)
1. Selecciona par (RP2040–ESP32, RP2040–Nano, ESP32–Nano).  
2. Configura baud (p. ej. 115200 y el **máximo estable** encontrado).  
3. Corre **1000 mensajes** (payload fijo, p. ej. 16 bytes).  
4. Guarda CSV en `assets/logs/uart/` con este **formato**:

