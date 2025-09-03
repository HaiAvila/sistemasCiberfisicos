---
title: "Pruebas"
parent: "UART"
nav_order: 3
layout: default
---

# Procedimiento de pruebas (UART)

## Parte A — Baudrate máximo
1. Conecta un par de placas.
2. Incrementa baud: 115200 → 230400 → 460800 → 921600 → 1M.  
3. Elige el mayor valor con 0% de pérdidas.

## Parte B — Latencia (1000 mensajes)
1. Configura baud fijo (ej. 38400 y el máximo estable encontrado).  
2. Corre 1000 mensajes con eco inmediato.  
3. Guarda los resultados en CSV con columnas:

timestamp_start_ms, timestamp_end_ms, rtt_ms, placa_A, placa_B, baud, ok


## Archivo de logs
Ejemplo de nombre:  
`uart_rp2040-esp32_38400_001.csv`

## Validación
- `ok` ≥ 99.9%  
- RTT sin valores imposibles (ej. 0 ms).
