---
layout: default
title: Protocolo UART
nav_exclude: true
has_children: true
nav_order: 3
has_toc: false
---

# Protocolo UART
En esta sección se documentan las pruebas realizadas con el protocolo **UART (Universal Asynchronous Receiver/Transmitter)** utilizando tres plataformas de sistemas embebidos: **Arduino Nano (ATmega328P)**, **XIAO RP2040** y **ESP32**.  

El objetivo fue:  
1. **Identificar el baud rate máximo estable** alcanzado en cada plataforma.  
2. **Medir la latencia promedio** en la comunicación entre pares de dispositivos, enviando y recibiendo **1000 mensajes** por prueba.  
3. Representar los resultados mediante **gráficas**, **tablas comparativas**, y reforzar con **diagramas eléctricos** y **códigos implementados**.  

Los apartados que siguen detallan la descripción breve del protocolo, la metodología utilizada y los resultados experimentales.

- [Descripción UART]({{ "/descripcionUart" | relative_url }})
- [Resultados]({{ "/resultadosUart" | relative_url }})
- [Códigos]({{ "/codigosUart" | relative_url }})
- [Videos]({{ "/videosUart" | relative_url }})
