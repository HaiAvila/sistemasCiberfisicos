---
title: "Resultados"
parent: "UART"
nav_order: 4
---

# Resultados de latencia (UART)

Se midió la latencia round-trip enviando **1000 mensajes** a **38400 baud** en cada par de plataformas.

## Resumen comparativo

| Par | Baud | n | Prom (ms) | p50 | p90 | p99 | σ | Min | Max | Outliers |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Arduino ↦ RP2040 | 38400 | 1000 | 3.197 | 3.192 | 3.204 | 3.216 | 0.152 | 3.168 | 8.004 | 1 |
| RP2040 ↦ Arduino | 38400 | 1000 | 2.981 | 2.971 | 2.997 | 3.044 | 0.208 | 2.919 | 8.345 | 2 |
| Arduino ↦ ESP32 | 38400 | 1000 | — | — | — | — | — | — | — | — | — |
| ESP32 ↦ Arduino | 38400 | 1000 | — | — | — | — | — | — | — | — | — |
| ESP32 ↦ RP2040 | 38400 | 1000 | — | — | — | — | — | — | — | — | — |
| RP2040 ↦ ESP32 | 38400 | 1000 | — | — | — | — | — | — | — | — | — |

---

## Detalle por par

### Arduino ↦ RP2040
![Latencia Arduino → RP2040](/assets/img/uart/resultados/Ard_RP.jpeg)

- Promedio ~3.2 ms, un pico aislado ~8 ms.

### RP2040 ↦ Arduino
![Latencia RP2040 → Arduino](/assets/img/uart/resultados/RP_Ard.jpeg)

- Mejor promedio (~2.98 ms), dos picos aislados ~8.3 ms.

*(Agrega aquí las demás gráficas cuando las subas).*

---

## Discusión
- **RP2040 → Arduino** mostró la menor latencia promedio.  
- Los picos de ~8 ms sugieren jitter por ISR o buffers.  
- Conexiones estables a 38400 baud, nivelado de voltaje necesario con Nano.
