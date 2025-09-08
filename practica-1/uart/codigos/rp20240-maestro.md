---
title: "RP2040 — Maestro"
parent: "Firmware"
grand_parent: "UART"
layout: default
nav_order: 3
---

# RP2040 — Maestro

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~python
# rp2040_uart_master.py  (XIAO RP2040 - MicroPython)
from machine import UART, Pin
import time

DUT_BAUD   = 38400
N_ITER     = 1000
TIMEOUT_MS = 1000
FILENAME   = f"latencia_uart_{DUT_BAUD}.csv"
LOG_TIMEOUT_AS_EMPTY = True  # True: deja la columna vacía; False: "TIMEOUT"

# XIAO RP2040: UART0 -> TX=P0 (D6), RX=P1 (D7)
uart = UART(0, baudrate=DUT_BAUD, tx=Pin(0), rx=Pin(1))

def read_line_timeout(timeout_ms=TIMEOUT_MS):
    t0 = time.ticks_ms()
    buf = bytearray()
    while time.ticks_diff(time.ticks_ms(), t0) < timeout_ms:
        if uart.any():
            c = uart.read(1)
            if not c: 
                continue
            if c == b'\r':
                continue
            if c == b'\n':
                return bytes(buf)
            if len(buf) < 16:
                buf.extend(c)
        time.sleep_us(50)
    return None

def run_test():
    value = 1
    valid = 0; mean = 0.0; M2 = 0.0

    with open(FILENAME, "w") as f:
        f.write("index,rtt_us\n")

        for idx in range(1, N_ITER + 1):
            msg = f"{value:04d}\n"
            t0 = time.ticks_us()
            uart.write(msg)

            line = read_line_timeout()
            if line is None:
                f.write(f"{idx},\n" if LOG_TIMEOUT_AS_EMPTY else f"{idx},TIMEOUT\n")
                if (idx % 50) == 0: f.flush()
                print(idx, "TIMEOUT")
                continue

            t1 = time.ticks_us()
            rtt = time.ticks_diff(t1, t0)
            f.write(f"{idx},{rtt}\n")
            if (idx % 50) == 0: f.flush()
            print(idx, rtt)

            # estadísticas online (Welford)
            valid += 1
            x = float(rtt)
            d  = x - mean
            mean += d / valid
            M2   += d * (x - mean)

            # siguiente valor = respuesta del esclavo (echo+1)
            try:
                value = int(line.decode().strip())
            except:
                value = value + 1

    if valid >= 2:
        var = M2 / (valid - 1)
        print("DONE,", FILENAME)
        print("VALID,", valid)
        print("AVG_US,", round(mean, 3))
        print("STDDEV_US,", round(var**0.5, 3))
    else:
        print("DONE,", FILENAME)
        print("VALID,0")

run_test()
~~~
{% endraw %}