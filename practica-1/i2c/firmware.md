---
title: "Firmware"
parent: "I2C"
nav_order: 2
layout: default
has_children: true
---

# Firmware — convenciones y códigos

## Convenciones de la práctica
- **Frecuencia I2C:** 100 kHz  
- **Formato de mensaje:** entero de 4 dígitos + salto de línea (`"%04d\n"`)  
- **Roles:**  
  - *Maestro* → inicia y mide RTT  
  - *Esclavo* → responde eco+1 inmediato  
- **Conexiones:** SDA y SCL compartidos, GND común

---

## Códigos por plataforma

1. **Arduino Nano**
   - [Maestro]({{ "/practica-1/i2c/codigos/nano-maestro/" | relative_url }})
   - [Esclavo]({{ "/practica-1/i2c/codigos/nano-esclavo" | relative_url }})

2. **RP2040**
   - [Maestro]({{ "/practica-1/i2c/codigos/rp2040-maestro" | relative_url }})
   - [Esclavo]({{ "/practica-1/i2c/codigos/rp2040-esclavo" | relative_url }})

3. **ESP32**
   - [Maestro]({{ "/practica-1/i2c/codigos/esp32-maestro" | relative_url }})
   - [Esclavo]({{ "/practica-1/i2c/codigos/esp32-esclavo" | relative_url }})
