---
title: "Plataformas y protocolos"
nav_order: 3
layout: default
has_toc: true
---

# Plataformas y protocolos (visión general)

Esta página resume las **características clave** de las placas usadas y lo esencial de los **protocolos UART, I2C y SPI** para que cualquiera pueda replicar las conexiones sin ser experto.

> **Objetivo práctico:** dejar definidos **pines sugeridos**, **niveles lógicos** y **precauciones** para evitar errores típicos (falta de GND común, pull-ups en I2C, sobre-voltaje 5V→3.3V, etc.).

---

## 1) Placas utilizadas

### Arduino Nano (ATmega328P, 5 V)
- **CPU / Reloj:** 8-bit @ 16 MHz  
- **Memoria:** 32 KB Flash (≈2 KB bootloader), 2 KB SRAM, 1 KB EEPROM  
- **I/O:** 22 digitales (6 PWM), 8 analógicas  
- **Periféricos HW:** 1×UART, 1×I2C (TWI), 1×SPI  
- **Nivel lógico:** **5 V**
- **Notas prácticas:** UART HW está compartido con USB-serial (D0/D1)

**Pines sugeridos para esta práctica**

| Protocolo | Pines Nano | Comentario |
|-----------|------------|------------|
| UART | D1 (TX), D0 (RX) | Conectado al USB-serial |
| I2C  | A4 (SDA), A5 (SCL) | Requiere pull-ups |
| SPI  | D13 (SCK), D11 (MOSI), D12 (MISO), **D10 (SS)** | SS como CS/SS |


---

### Seeed XIAO **RP2040** (3.3 V)
- **CPU / Reloj:** 2×Cortex-M0+ @ hasta 133 MHz  
- **Memoria:** 264 KB SRAM, 2 MB Flash a bordo  
- **Periféricos HW:** **2×UART**, **2×I2C**, **2×SPI**, PIO  
- **Nivel lógico:** **3.3 V**
- **GPIO en la placa XIAO:** 11 GPIO útiles

**Pines sugeridos (ajusta si tu serigrafía varía)**

| Protocolo | Pines RP2040 | Comentario |
|-----------|--------------|------------|
| **UART**  | `GP0 (TX)`, `GP1 (RX)` | UART0 |
| **I2C**   | `GP4 (SDA)`, `GP5 (SCL)` | I2C0 + pull-ups |
| **SPI**   | `GP18 (SCK)`, `GP19 (MOSI)`, `GP16 (MISO)`, **`GP17 (CS)`** | SPI0 |


---

### ESP32 DevKit (3.3 V)
- **CPU / Reloj:** Dual-core @ 160–240 MHz  
- **Memoria:** ~520 KB SRAM internos, 4 MB Flash típica  
- **Periféricos HW:** **3×UART**, **2×I2C**, **2×SPI (HSPI/VSPI)**, WiFi/BLE  
- **Nivel lógico:** **3.3 V** 

**Pines sugeridos**

| Protocolo | Pines ESP32 | Comentario |
|:---------:|-------------|------------|
| **UART**  | `GPIO1 (TX0)`, `GPIO3 (RX0)` | O usa UART2: `GPIO17 (TX)`, `GPIO16 (RX)` |
| **I2C**   | `GPIO21 (SDA)`, `GPIO22 (SCL)` | Requiere pull-ups |
| **SPI**   | `GPIO18 (SCK)`, `GPIO23 (MOSI)`, `GPIO19 (MISO)`, **`GPIO5 (CS)`** | VSPI |


> **Precaución de niveles:** En **I2C**, utiliza **pull-ups a 3.3 V**.

---

## 2) Protocolos de comunicación (qué, cuándo y cómo)

| Protocolo | Líneas | Topología | Velocidad típica | Dirección | Pros | Contras |
|---|---|---|---|---|---|---|
| **UART** | TX, RX (+GND) | Punto a punto | 9 600 – 1 000 000 baudios | Sin direcciones | Simple, muy común, fácil de depurar | No multi-nodo, timing sensible |
| **I2C** | SDA, SCL (+GND) | Bus compartido maestro–múltiples esclavos | 100 k / 400 k (hasta 1 MHz en fast mode+) | Por dirección | Solo 2 cables, multi-nodo, hot-swap | Requiere **pull-ups**, más ruido/latencia |
| **SPI** | SCK, MOSI, MISO, **CS** (+GND) | Maestro–varios esclavos (CS por dispositivo) | 1–10+ MHz (según placa/cables) | Por línea **CS** | Muy rápido, full-duplex | Más cables, un **CS** por esclavo |

**Checklist por protocolo**
- **UART:** TX↔RX cruzados, misma configuración (baud/paridad/stop), GND común.  
- **I2C:** **pull-ups** en SDA/SCL, dirección correcta, GND común, cable corto.  
- **SPI:** define **CS** por esclavo, misma **polaridad/fase** (modo SPI), GND común.

> **Consejo:** siempre empieza probando **solo 1 enlace** (p. ej., RP2040↔ESP32 por UART), valida eco y luego escala a I2C/SPI.

---

## 3) Qué archivos y dónde?

- **Códigos** por placa → `----`  
- **Esquemas** (PNG/SVG exportados) → `----`  
- **Fotos** de conexiones reales → `----`  
- **Logs CSV** de cada prueba → `----`  
- **Videos cortos** de demostración → `----`
