---
title: "Resultados"
parent: "Protocolo UART"
nav_order: 2
layout: default
has_toc: true
---

# Latencia y resultados (UART)

En esta sección se reportan las mediciones de **latencia round-trip** entre pares de dispositivos con **baud fijo = 38400**. Para cada par se ejecutaron **1000 mensajes** (ping→eco inmediato→medición en el emisor).  
Métricas: **promedio**, **Media aritmética**, **p90**, **p99**, **σ**, **mín**, **máx**, y **outliers** (> media + 3σ).

> Nota de limpieza: se descartaron valores imposibles (p. ej., 0 ms).

## Resumen comparativo

| Par | Baud | n | Prom (ms) | p50 | p90 | p99 | σ | Min | Max | Outliers |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Arduino ↦ RP2040 | 38400 | 1000 | **3.197** | 3.192 | 3.204 | 3.216 | 0.152 | 3.168 | 8.004 | 1 |
| RP2040 ↦ Arduino | 38400 | 1000 | **2.981** | 2.971 | 2.997 | 3.044 | 0.208 | 2.919 | 8.345 | 2 |
| Arduino ↦ ESP32 | 38400 | 1000 | — | — | — | — | — | — | — | — |
| ESP32 ↦ Arduino | 38400 | 1000 | — | — | — | — | — | — | — | — |
| ESP32 ↦ RP2040 | 38400 | 1000 | — | — | — | — | — | — | — | — |
| RP2040 ↦ ESP32 | 38400 | 1000 | — | — | — | — | — | — | — | — |

> Interpretación: en tus pruebas, **RP2040 → Arduino** mostró el menor promedio (~2.98 ms). En ambos sentidos aparecen picos aislados (~8 ms), típicos de jitter del SO/driver/buffer o de un eco que se retrasó por interrupciones.

**Cómo leer la tabla**

- **Par**: combinación de dispositivos probados (p. ej., Arduino y RP2040).
- **Baud**: velocidad configurada en UART (baudios). En nuestras pruebas: **38400**.
- **n**: número de mensajes válidos utilizados en las métricas (tras limpieza).
- **Prom (ms)**: **media aritmética** de la latencia round-trip en milisegundos.
- **p50 (ms)**: **percentil 50** = **mediana** (el 50% de las muestras ≤ este valor).  
- **p90 (ms)**: **percentil 90** (el 90% de las muestras ≤ este valor).  
- **p99 (ms)**: **percentil 99** (el 99% de las muestras ≤ este valor; proxy de “casi peor caso”).  
  > Útil para ver colas/lags: cuanto más bajo p99, más predecible el enlace.
- **σ (ms)**: **desviación estándar**; dispersión alrededor de la media (menor = más estable).
- **Min / Max (ms)**: latencias mínima y máxima observadas.
- **Outliers**: conteo de muestras atípicas (definidas aquí como > **media + 3σ**). 


## Detalle por par

### Arduino ↦ RP2040  {#uart-arduino-rp2040}
- **Métricas**: Prom 3.197 ms | p50 3.192 | p90 3.204 | p99 3.216 | σ 0.152 | **1** outlier (> 3.654 ms).  
- **Gráfica**: inserta aquí tu imagen  
  `![Latencia](Imagenes/Ard_RP.jpeg)`  
- **Notas**: cable TX↔RX cruzado + GND común; diferencia de niveles (5V↦3.3V) resuelta con conversor o divisor.

### RP2040 ↦ Arduino  {#uart-rp2040-arduino}
- **Métricas**: Prom 2.981 ms | p50 2.971 | p90 2.997 | p99 3.044 | σ 0.208 | **2** outliers (> 3.605 ms).  
- **Gráfica**: inserta aquí tu imagen  
  `![Latencia](Imagenes/)`  
- **Notas**: mismas condiciones; picos esporádicos (~8 ms) probablemente por cola de transmisión o ISR.

### Arduino ↦ ESP32  {#uart-arduino-esp32}
- **Métricas**: *(pendiente de tu equipo)*  
- **Gráfica**: `Imagenes/UART_Arduino_ESP32_latencia.png`

### ESP32 ↦ Arduino  {#uart-esp32-arduino}
- **Métricas**: *(pendiente)*  
- **Gráfica**: `Imagenes/UART_ESP32_Arduino_latencia.png`

### ESP32 ↦ RP2040  {#uart-esp32-rp2040}
- **Métricas**: *(pendiente)*  
- **Gráfica**: `Imagenes/UART_ESP32_RP2040_latencia.png`

### RP2040 ↦ ESP32  {#uart-rp2040-esp32}
- **Métricas**: *(pendiente)*  
- **Gráfica**: `Imagenes/UART_RP2040_ESP32_latencia.png`

## Metodología (resumen)
- **Trama**: texto corto con contador + checksum; eco inmediato en el receptor.  
- **Medición**: `Δt = T1 - T0` en el emisor (μs/ms).  
- **Criterio de estabilidad**: 1000/1000 mensajes válidos (checksum OK).  
- **Baud**: **38400** fijo para comparabilidad.  
- **Limpieza**: se ignoran muestras imposibles (p. ej. 0 ms) y se reportan outliers.

## Datos y material complementario
- **CSV/TXT** (opcional): puedes enlazar con `[Descargar CSV](assets/datos/uart_arduino_rp2040_38400.csv)`.  
- **Videos**: ver la página *Videos*.  
- **Códigos**: ver la página *Códigos*.

