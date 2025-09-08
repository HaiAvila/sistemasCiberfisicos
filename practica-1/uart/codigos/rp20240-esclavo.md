---
title: "RP2040 — Esclavo"
parent: "Firmware"
grand_parent: "UART"
layout: default
nav_order: 4
---

# RP2040 — Esclavo

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~python
# rp2040_uart_slave.py  (Seeed XIAO RP2040 - MicroPython)
# Esclavo UART: recibe "%04d\n" y responde inmediatamente con (valor+1) en el mismo formato.

from machine import UART, Pin
import time

BAUD = 38400
# XIAO RP2040: UART0 -> TX=P0 (D6), RX=P1 (D7)
uart = UART(0, baudrate=BAUD, tx=Pin(0), rx=Pin(1))

buf = bytearray()
while True:
    if uart.any():
        c = uart.read(1)
        if not c:
            continue
        if c == b'\r':
            continue
        if c == b'\n':
            # Procesa línea completa
            try:
                v = int(bytes(buf).decode().strip()) + 1
            except:
                v = 0
            uart.write(f"{v:04d}\n")
            buf = bytearray()
        else:
            if len(buf) < 16:
                buf.extend(c)
    else:
        time.sleep_us(50)
~~~
{% endraw %}