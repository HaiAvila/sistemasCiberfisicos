---
title: "Arquitectura"
parent: "Arq.2 — Multipunto a Punto"
layout: default
nav_order: 1
---

# Arquitectura (Ejercicio 5)

**Descripción**  
- **Device A:** contiene dos switches de entrada (`Switch1`, `Switch2`).  
- **Device B:** controla `LED1`.  
- **Device C:** controla `LED2`.  
- El dispositivo maestro es **A**, que envía comandos a B y C.  

**Protocolos a implementar**
- A ↔ B con **I2C**.  
- A ↔ C con **UART**.  
- Alternativa: implementación con **SPI**.  

**Diagrama de bloques**
![Conexión]({{ "/assets/img/arq2.png" | relative_url }})
