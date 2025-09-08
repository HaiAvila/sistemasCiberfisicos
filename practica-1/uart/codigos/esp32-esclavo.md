---
title: "ESP32 — Esclavo"
parent: "Firmware"
grand_parent: "UART"
layout: default
nav_order: 6
---

# ESP32 — Esclavo

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~c++
// NANO_INIT_UART_RTT_FIXED.ino
print()
~~~
{% endraw %}