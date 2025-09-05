---
title: "Pruebas"
parent: "I2C"
nav_order: 3
layout: default
---

# Procedimiento de pruebas (I2C)

## Latencia (1000 mensajes)
1. Configura frecuencia fija (100 kHz).  
2. Corre 1000 mensajes con eco inmediato.  
3. Guarda los resultados en CSV con columnas:

timestamp_start_ms, timestamp_end_ms, rtt_ms, counter_i
