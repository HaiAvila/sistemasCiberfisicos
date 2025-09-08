---
title: "ESP32 — Maestro"
parent: "Firmware"
grand_parent: "UART"
layout: default
nav_order: 5
---

# ESP32 — Maestro

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~python
# esp32_uart_master.py  (ESP32 - Micropython)
from machine import UART
import time

# UART2 en ESP32: TX=17, RX=16 (cablea 17→RX del RP2040 y 16←TX del RP2040)
uart = UART(2, baudrate=9600, tx=17, rx=16, timeout=0)  # timeout=0 = no bloqueante

def read_exact(n, timeout_ms=500):
    """Lee exactamente n bytes o None si expira timeout."""
    buf = b""
    t0 = time.ticks_ms()
    while len(buf) < n and time.ticks_diff(time.ticks_ms(), t0) < timeout_ms:
        if uart.any():
            chunk = uart.read(n - len(buf)) or b""
            if chunk:
                buf += chunk
        time.sleep_ms(1)  # ceder CPU
    return buf if len(buf) == n else None

with open("latencia_rp_espera_esp.txt", "w") as f:
    time.sleep_ms(200)  # pequeño warm-up

    for i in range(1001):
        msg = f"{i:04d}".encode()

        # Vaciar residuos antes de cada envío
        while uart.any():
            uart.read()

        t0 = time.ticks_us()
        uart.write(msg)

        resp = read_exact(4, timeout_ms=500)  # esperamos 4 bytes exactos
        t1 = time.ticks_us()

        if resp is not None:
            esperado = f"{i+1:04d}".encode()
            if resp == esperado:
                lat_ms = time.ticks_diff(t1, t0) / 1000.0
                f.write(f"{lat_ms}\n")
                print(f"{lat_ms:.3f}")
            else:
                f.write("-2\n")
                print(f"ENVIADO: {msg.decode()} | RECIBIDO: {resp.decode()} (MISMATCH)")
        else:
            f.write("-1\n")
            print(f"ENVIADO: {msg.decode()} | RECIBIDO: None (TIMEOUT)")

        # Detener al recibir 1001 (i=1000 debe devolver 1001)
        if resp == b"1001":
            print("Fin del test.")
            break

        time.sleep_ms(20)  # breve respiro para estabilidad
~~~
{% endraw %}