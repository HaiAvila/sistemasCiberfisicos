---
title: "Protocolo I2C"
parent: "Práctica 1"
nav_exclude: true
nav_order: 4
layout: default
has_children: true
has_toc: false
---

# Protocolo I2C

En esta sección se documentan las pruebas con **I2C** entre **Arduino Nano (ATmega328P)**, **XIAO RP2040** y **ESP32**. El objetivo es **medir la latencia** entre pares de dispositivos a una **frecuencia de bus fija** y presentar **6 gráficas** (todas las combinaciones de a dos). También se incluyen **diagramas de arquitectura**, **esquemáticos** y **código**.

**Parámetros de referencia (sugeridos):**  
- **Frecuencia I2C (SCL):** `100 kHz` (si su equipo acordó `400 kHz`, manténganlo constante en todas las pruebas).  
- **Carga útil:** entero pequeño (p. ej., de 0–9999). Puede enviarse en **ASCII** (`"0001"`) o como **binario** (2 bytes). Lo importante es que el **esclavo responda valor+1** de manera inmediata.  
- **Repeticiones:** `n = 1000` por par.  
- **Métrica:** latencia **round-trip** medida en el **maestro** (promedio, p50, p90, p99, σ, min, max, outliers).

**Metodología (resumen):**  
1) **Rol maestro** inicia una operación **write** con el valor actual y seguidamente realiza un **read** del mismo esclavo (eco+1).  
2) **Δt = T₁ − T₀** donde `T₀` se toma justo antes del `write` y `T₁` cuando se recibe el valor leído.  
3) Repetir **1000** veces por par, registrar CSV (`index,latency_us` o `index,latency_ms`).  
4) Graficar histograma/boxplot por par y reportar tabla resumen (prom, p50, p90, p99, σ, min, max, outliers).

**Pares de prueba (6):**  
- Arduino ↔ ESP32  
- Arduino ↔ RP2040  
- ESP32 ↔ RP2040  
*(Si midieron direcciones separadas, etiquetar A→B y B→A en la tabla.)*

**Montaje y seguridad eléctrica:**  
- **Líneas:** `SDA`, `SCL` y **GND común**.  
- **Pull-ups obligatorios** en `SDA/SCL` (típ.: **4.7 kΩ** a **3.3 V**).  
- **Niveles lógicos:** mezclar **5 V** (Nano) con **3.3 V** (ESP32/RP2040) requiere **level shifter** o que las **pull-ups** estén a **3.3 V** con transistores/bidireccional; **no** tirar `SDA/SCL` a 5 V si hay dispositivos a 3.3 V.  
- Mantener **longitud de cables** similar en todos los pares.

**Convenciones de archivos:**  
- Imágenes: `Imagenes/I2C_<A>_<B>_latencia.png`  
- CSV: `datos/i2c_<a>_<b>_<freq>.csv`  
- Anclas para subsecciones: `#i2c-arduino-esp32`, `#i2c-arduino-rp2040`, `#i2c-esp32-rp2040`.

> En las páginas hijas encontrarás: **Descripción de I2C**, **resultados (I2C)**, **Códigos (I2C)** y **Videos (I2C)**.



