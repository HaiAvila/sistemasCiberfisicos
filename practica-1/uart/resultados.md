---
title: "Resultados"
parent: "UART"
nav_order: 4
layout: default
---

# Resultados de latencia (UART)

Se midió la latencia round-trip enviando **1000 mensajes** a **38400 baud** en cada par de plataformas.

## Resumen comparativo

| Par | Baud | n | Prom (ms) | p50 | p90 | p99 | σ | Min | Max | Outliers |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Arduino ↦ RP2040 | 38400 | 1000 | 3.197 | 3.197 | 3.204 | 3.216 | 0.152 | 3.168 | 8.004 | 1 |
| RP2040 ↦ Arduino | 38400 | 1000 | 2.981 | 2.981 | 2.997 | 3.044 | 0.208 | 2.919 | 8.345 | 2 |
| Arduino ↦ ESP32 | 38400 | 1000 | 2.712 | 2.712 | 2.716 | 2.720 | 0.006 | 2.696 | 2.780 | 4 |
| ESP32 ↦ Arduino | 38400 | 1000 | 2.800 | 2.754 | 2.761 | 2.765 | 0.006 | 2.727 | 2.770 | 3 |
| ESP32 ↦ RP2040 | 38400 | 1000 | — | — | — | — | — | — | — | — |
| RP2040 ↦ ESP32 | 38400 | 1000 | — | — | — | — | — | — | — | — |

---

## Detalle por par

### Arduino ↦ RP2040
![Latencia Arduino → RP2040](/assets/img/Ard_RP.jpeg)
- Promedio ~3.2 ms.

### RP2040 ↦ Arduino
![Latencia RP2040 → Arduino](/assets/img/RP_Ard.jpeg)
- Promedio (~2.98 ms).

### ESP32 ↦ Arduino
![Latencia ESP32 → Arduino](/assets/img/uart_latency_ESP32_to_Nano_38400_20250820_171819_latency.png)
- Promedio ~2.8 ms.

### Arduino ↦ ESP32
![Latencia Arduino → ESP32](/assets/img/uart_latency_Nano_to_ESP32_38400_20250820_162831_latency.png)
- Promedio (~2.71 ms).

### ESP32 ↦ RP2040
![Latencia ESP32 → RP2040](/assets/img/IMAGEN_DANY)
- Promedio ~3.2 ms.

### RP2040 ↦ ESP32
![Latencia RP2040 → ESP32](/assets/img/IMAGEN_DANY)
- Promedio (~2.98 ms).

---

## Discusión
- **RP2040 → Arduino** mostró la menor latencia promedio.  
- Los picos de ~8 ms sugieren jitter por ISR o buffers.  
- Conexiones estables a 38400 baud, nivelado de voltaje necesario con Nano.
