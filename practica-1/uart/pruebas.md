---
title: "Pruebas"
parent: "UART"
nav_order: 3
layout: default
---

# Procedimiento de pruebas (UART)

## Latencia (1000 mensajes)
1. Configura baud fijo (38400).  
2. Corre 1000 mensajes con eco inmediato.  
3. Guarda los resultados en CSV con columnas:

timestamp_start_ms, timestamp_end_ms, rtt_ms, counter_i
