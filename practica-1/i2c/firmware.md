---
title: "Firmware"
parent: "I2C"
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
   - [Maestro]({{ "/practica-1/uart/codigos/nano-maestro.md" | relative_url }})
   - [Esclavo]({{ "/practica-1/uart/codigos/nano-esclavo.md" | relative_url }})

2. **RP2040**
   - [Maestro]({{ "/practica-1/uart/codigos/rp2040-maestro.md" | relative_url }})
   - [Esclavo]({{ "/practica-1/uart/codigos/rp2040-esclavo.md" | relative_url }})

3. **ESP32**
   - [Maestro]({{ "/practica-1/uart/codigos/esp32-maestro.md" | relative_url }})
   - [Esclavo]({{ "/practica-1/uart/codigos/esp32-esclavo.md" | relative_url }})
