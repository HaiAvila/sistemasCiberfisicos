---
title: "Arquitectura"
parent: "Ejercicio 4 — Punto a Multipunto"
layout: default
nav_order: 1
---

# Arquitectura (Ejercicio 4)

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
![Diagrama de bloques](/assets/img/arquitecturas/ej4/diagrama_bloques.png)

**Esquema eléctrico (KiCad)**  
- Proyecto: `/kicad/ej4_p2m.kicad_pro`
