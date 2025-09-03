---
title: "Firmware"
parent: "UART"
nav_order: 2
layout: default
---

# Firmware — convenciones y códigos

## Convenciones de la práctica
- **Baud:** 38400  
- **Formato de trama:** entero de 4 dígitos + salto de línea (`"%04d\n"`)  
- **Roles:**  
  - *Maestro* → inicia ping y mide RTT  
  - *Esclavo* → responde eco+1 inmediato  
- **Conexiones:** TX↔RX cruzados, GND común

---

## Arduino Nano
### Maestro — `nano_uart_master_rtt.ino`
*(código que imprime CSV `index,rtt_us` con 1000 iteraciones)*

### Esclavo — `nano_uart_slave_echo.ino`
*(código que responde eco+1 en el mismo formato)*

---

## RP2040
*(Aquí pegas tus códigos de maestro/esclavo en RP2040, siguiendo la misma convención)*

---

## ESP32
*(Aquí pegas tus códigos de maestro/esclavo en ESP32)*
