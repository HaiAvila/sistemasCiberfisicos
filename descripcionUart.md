---
layout: default
title: Descripción de UART
nav_order: 1
parent: "Protocolo UART"
---

# Descripción breve de UART

El protocolo **UART** es un método de comunicación serial asíncrona que permite la transmisión de datos entre dos dispositivos sin necesidad de compartir una señal de reloj.  

Características principales:  
- **Asíncrono:** utiliza bits de inicio y parada para sincronizar la comunicación.  
- **Transmisión punto a punto:** comúnmente entre dos nodos (TX ↔ RX).  
- **Full-duplex:** permite enviar y recibir datos al mismo tiempo.  
- **Conexión física mínima:** requiere únicamente tres líneas (TX, RX y GND).  
- **Velocidades típicas (baud rate):** desde 9600 hasta varios megabaudios, dependiendo del hardware.  

En esta práctica, el protocolo UART se utilizó como primera prueba de desempeño en comunicación entre sistemas embebidos, sirviendo como referencia frente a I2C y SPI.
