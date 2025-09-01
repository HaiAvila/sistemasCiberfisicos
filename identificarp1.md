---
layout: home
title: Identificar
nav_order: 2
---

En este apartado se identifican las principales características de las plataformas de sistemas embebidos utilizadas en la práctica: **Arduino Nano, XIAO RP2040 y ESP32**.  
Se consideran parámetros como **frecuencia de reloj, memoria, entradas/salidas (I/O) y protocolos de comunicación disponibles**.

## Arduino Nano
- **Frecuencia**: 16 MHz (ATmega328P).
- **Memoria**:  
  - Flash: 32 KB (2 KB usados por bootloader).  
  - SRAM: 2 KB.  
  - EEPROM: 1 KB.  
- **I/O**: 22 pines de E/S digitales (6 con PWM), 8 entradas analógicas.  
- **Protocolos**: UART, SPI, I2C.

## XIAO RP2040
- **Frecuencia**: 133 MHz (dual-core Cortex-M0+).  
- **Memoria**:  
  - Flash: 2 MB integrada.  
  - SRAM: 264 KB.  
- **I/O**: 11 pines GPIO, 4 ADC, hasta 9 PWM, I2S.  
- **Protocolos**: UART, SPI, I2C, USB.

## ESP32
- **Frecuencia**: 160 – 240 MHz (dual-core Xtensa LX6).  
- **Memoria**:  
  - SRAM: ~520 KB internos.  
  - Flash: 4 MB (dependiendo del módulo).  
- **I/O**: 34 GPIO (dependiendo del módulo), múltiples ADC, DAC, PWM.  
- **Protocolos**: UART, SPI, I2C, I2S, CAN, Ethernet, WiFi, Bluetooth.

---

### Comparación General

| Plataforma     | Frecuencia      | Memoria Flash | SRAM   | I/O principales         | Protocolos |
|----------------|----------------|---------------|--------|-------------------------|------------|
| Arduino Nano   | 16 MHz         | 32 KB         | 2 KB   | 22 GPIO, 6 PWM, 8 ADC   | UART, SPI, I2C |
| XIAO RP2040    | 133 MHz        | 2 MB          | 264 KB | 11 GPIO, 4 ADC, 9 PWM   | UART, SPI, I2C, USB |
| ESP32          | 160–240 MHz    | 4 MB          | 520 KB | 34 GPIO, ADC, DAC, PWM  | UART, SPI, I2C, I2S, CAN, WiFi, BT |

---