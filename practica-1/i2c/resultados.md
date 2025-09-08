---
title: "Resultados"
parent: "I2C"
nav_order: 4
layout: default
---

# Resultados de latencia (I2C)

Se midió la latencia round-trip enviando **1000 mensajes** a **100 kHz** en cada par de plataformas.

## Resumen comparativo

| Par | Frecuencia | n | Prom (ms) | p50 | p90 | p99 | σ | Min | Max | Outliers |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Arduino ↦ RP2040 | 100 kHz | 1000 | 3.197 | 3.197 | 3.204 | 3.216 | 0.152 | 3.168 | 8.004 | 1 |
| RP2040 ↦ Arduino | 100 kHz | 1000 | 2.981 | 2.981 | 2.997 | 3.044 | 0.208 | 2.919 | 8.345 | 2 |
| Arduino ↦ ESP32 | 100 kHz | 1000 | 2.712 | 2.712 | 2.716 | 2.720 | 0.006 | 2.696 | 2.780 | 4 |
| ESP32 ↦ Arduino | 100 kHz | 1000 | 2.800 | 2.754 | 2.761 | 2.765 | 0.006 | 2.727 | 2.770 | 3 |
| ESP32 ↦ RP2040 | 100 kHz | 1000 | — | — | — | — | — | — | — | — |
| RP2040 ↦ ESP32 | 100 kHz | 1000 | — | — | — | — | — | — | — | — |

---

## Detalle por par

### Arduino ↦ RP2040
![Latencia Arduino → RP2040]({{ "/assets/img/Ard_RP.jpeg" | relative_url }})
- Promedio ~3.2 ms.

### RP2040 ↦ Arduino
![Latencia RP2040 → Arduino]({{ "/assets/img/RP_Ard_I2C.jpeg" | relative_url }})
- Promedio ~2.98 ms.

### ESP32 ↦ Arduino
![Latencia ESP32 → Arduino]({{ "/assets/img/i2c_latency_ESP32_to_Nano_0_20250822_165652_latency.png" | relative_url }})
- Promedio ~2.8 ms.

### Arduino ↦ ESP32
![Latencia Arduino → ESP32]({{ "/assets/img/i2c_latency_NANO_to_ESP32_100000_20250825_172233_latency.png" | relative_url }})
- Promedio ~2.71 ms.

### ESP32 ↦ RP2040
![Latencia ESP32 → RP2040](/assets/img/i2c/IMAGEN_DANY)
- Promedio ~3.2 ms.

### RP2040 ↦ ESP32
![Latencia RP2040 → ESP32](/assets/img/i2c/IMAGEN_DANY)
- Promedio ~2.98 ms.

---

## Discusión
- A  
- B  
- C  
