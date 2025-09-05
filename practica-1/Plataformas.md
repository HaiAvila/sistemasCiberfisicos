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
- **Nivel lógico:** **5 V** (⚠️ no tolera 3.3→5 ni 5→3.3 sin adaptación)  
- **Notas prácticas:** UART HW está compartido con USB-serial (D0/D1)

**Pines sugeridos para esta práctica**
| Protocolo | Pines Nano | Comentario |
|---|---|---|
| UART | D1 (TX), D0 (RX) | Conectado al USB-serial |
| I2C | A4 (SDA), A5 (SCL) | Requiere pull-ups |
| SPI | D13 (SCK), D11 (MOSI), D12 (MISO), **D10 (SS)** | SS como CS/SS |

---

### Seeed XIAO **RP2040** (3.3 V)
- **CPU / Reloj:** 2×Cortex-M0+ @ hasta 133 MHz  
- **Memoria:** 264 KB SRAM, 2 MB Flash a bordo  
- **Periféricos HW:** **2×UART**, **2×I2C**, **2×SPI**, PIO  
- **Nivel lógico:** **3.3 V** (⚠️ **no** tolera 5 V)  
- **GPIO en la placa XIAO:** 11 GPIO útiles

**Pines sugeridos (ajusta si tu serigrafía varía)**
| Protocolo | Pines RP2040 | Comentario |
|---|---|---|
| UART | GP0 (TX), GP1 (RX) | UART0 |
| I2C | GP4 (SDA), GP5 (SCL) | I2C0 + pull-ups |
| SPI | GP18 (SCK), GP19 (MOSI), GP16 (MISO), **GP17 (CS)** | SPI0 |

---

### ESP32 DevKit (3.3 V)
- **CPU / Reloj:** Dual-core @ 160–240 MHz  
- **Memoria:** ~520 KB SRAM internos, 4 MB Flash típica  
- **Periféricos HW:** **3×UART**, **2×I2C**, **2×SPI (HSPI/VSPI)**, WiFi/BLE  
- **Nivel lógico:** **3.3 V** (⚠️ **no** tolera 5 V)

**Pines sugeridos (VSPI por comodidad)**
| Protocolo | Pines ESP32 | Comentario |
|---|---|---|
| UART | GPIO1 (TX0), GPIO3 (RX0) | O usa UART2: GPIO17 (TX), 16 (RX) |
| I2C | GPIO21 (SDA), GPIO22 (SCL) | Pull-ups |
| SPI | GPIO18 (SCK), GPIO23 (MOSI), GPIO19 (MISO), **GPIO5 (CS)** | VSPI |

> **Precaución de niveles:** Nano es **5 V**. Si lo conectas con RP2040/ESP32 (**3.3 V**) en UART o SPI, usa **divisor resistivo/level shifter** hacia las entradas de 3.3 V.  
> En **I2C**, utiliza **pull-ups a 3.3 V** y, de ser posible, un **translador MOSFET** si el bus mezcla 5 V y 3.3 V.

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

---

## 3) Pines que usaremos en la práctica (resumen rápido)

| Placa | UART | I2C | SPI |
|---|---|---|---|
| **Nano** | D1 (TX), D0 (RX) | A4 (SDA), A5 (SCL) | D13 (SCK), D11 (MOSI), D12 (MISO), **D10 (SS)** |
| **RP2040** | GP0 (TX), GP1 (RX) | GP4 (SDA), GP5 (SCL) | GP18 (SCK), GP19 (MOSI), GP16 (MISO), **GP17 (CS)** |
| **ESP32** | GPIO1 (TX), GPIO3 (RX) *(o 17/16)* | GPIO21 (SDA), GPIO22 (SCL) | GPIO18 (SCK), GPIO23 (MOSI), GPIO19 (MISO), **GPIO5 (CS)** |

> **Consejo:** siempre empieza probando **solo 1 enlace** (p. ej., RP2040↔ESP32 por UART), valida eco y luego escala a I2C/SPI.

---

## 4) Qué archivos y dónde (para que cualquiera replique)

- **Códigos** por placa → `assets/code/{rp2040,esp32,atmega328}/`  
- **Esquemas** (PNG/SVG exportados) → `assets/img/esquemas/`  
- **Fotos** de conexiones reales → `assets/img/plataformas/`  
- **Logs CSV** de cada prueba → `assets/logs/{uart,i2c,spi}/`  
- **Videos cortos** de demostración → `assets/video/{uart,i2c,spi}/`

Con esto, las siguientes secciones (UART/I2C/SPI) solo tienen que referenciar **esta tabla de pines** y usar los **mismos nombres** de carpetas.
