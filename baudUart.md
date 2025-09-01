---
layout: default
title: Baud rate máximo
nav_order: 2
parent: "Protocolo UART"
has_toc: true
---

# Baud rate máximo

Resumen de los mayores baud rates **estables** por plataforma y pares (criterio: 1000 mensajes sin pérdidas/corrupción y jitter aceptable).

| Par                     | Baud máx. estable | Notas (errores, nivel lógico, cables) |
|-------------------------|-------------------:|----------------------------------------|
| Arduino → ESP32         | …                  | …                                      |
| ESP32 → Arduino         | …                  | …                                      |
| Arduino → RP2040        | …                  | …                                      |
| RP2040 → Arduino        | …                  | …                                      |
| ESP32 → RP2040          | …                  | …                                      |
| RP2040 → ESP32          | …                  | …                                      |

> Criterio “estable”: 1000/1000 mensajes válidos (checksum OK) y jitter < 10% del promedio.

## Arduino → ESP32 {#uart-arduino-esp32}
Breve nota del montaje (TX/RX, conversión de nivel si aplica) y el **baud máximo estable encontrado**.  
- Intentados: 9600, 38400, 115200, 230400, 460800, 1M (si aplica)  
- Resultado: **XXXXX baud** estables  
- Evidencia: enlace a mini log / captura

## ESP32 → Arduino {#uart-esp32-arduino}
…

## Arduino → RP2040 {#uart-arduino-rp2040}
…

## RP2040 → Arduino {#uart-rp2040-arduino}
…

## ESP32 → RP2040 {#uart-esp32-rp2040}
…

## RP2040 → ESP32 {#uart-rp2040-esp32}
…
