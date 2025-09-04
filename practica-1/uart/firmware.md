---
title: "Firmware"
parent: "UART"
nav_order: 2
layout: default
has_children: true
---

# Firmware — convenciones y códigos

## Convenciones de la práctica
- **Baud:** 38400  
- **Formato de trama:** entero de 4 dígitos + salto de línea (`"%04d\n"`)  
- **Roles:**  
  - *Maestro* → inicia y mide RTT  
  - *Esclavo* → responde eco+1 inmediato  
- **Conexiones:** TX↔RX cruzados, GND común

---

## Códigos por plataforma

1. **Arduino Nano**
   - [Maestro]({{ "/uart/firmware/nano-maestro/" | relative_url }})
   - [Esclavo]({{ "/uart/firmware/nano-esclavo/" | relative_url }})

2. **RP2040**
   - [Maestro]({{ "/uart/firmware/rp2040-maestro/" | relative_url }})
   - [Esclavo]({{ "/uart/firmware/rp2040-esclavo/" | relative_url }})

3. **ESP32**
   - [Maestro]({{ "/uart/firmware/esp32-maestro/" | relative_url }})
   - [Esclavo]({{ "/uart/firmware/esp32-esclavo/" | relative_url }})
