---
title: "ESP32"
parent: "Códigos"
nav_order: 3
layout: default
has_toc: true
---

## ESP32
### Rol y formato

**Maestro:** inicia ping, mide **RTT** con `ticks_us()` y guarda **TXT** como `index,rtt_us` (**1000** iteraciones).  
**Esclavo:** responde **eco+1** inmediatamente al recibir `"%04d\n"`.  

**Baud / trama:** `38400`, `8N1`, `"%04d\n"`.  

**Pines (UART2 en ESP 32):** `TX2 = GPIO17 (G17)`, `RX2 = GPIO16 (G16)`.  

**Niveles lógicos:** ESP32 es **3.3 V**; el otro dispositivo es **3.3 V** (RP 2040).  

**Cableado:** `TX ↔ RX` cruzado y **GND común**.  

**Uso (maestro):** en **Thonny**, guardar y ejecutar `esp32_master_uart.py`; al terminar, descargar el TXT del dispositivo.  
**Uso (esclavo):** guardar como `main.py` (o ejecutar manualmente) y dejar corriendo mientras el maestro mide.

### Maestro — `esp32_uart_master.py`
**Plataforma:** ESP 32 (MicroPython)  
**Baud/trama:** `38400`, `8N1`  
**Pines (UART0):** `TX = GPIO17 (G17)`, `RX = GPIO16 (G16)`  
**Salida:** TXT en el dispositivo (`index,rtt_us`) con **1000** iteraciones
- **Validaciones rápidas:** si ves `TIMEOUT`, revisa **TX↔RX**, **GND**, mismo **baud** en ambos; evita cargas de CPU en el esclavo.

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

### Esclavo — `esp32_uart_slave.py`
**Plataforma:** ESP32 (C++)  
**Baud/trama:** `38400`, `8N1`  
**Pines (UART2 en ESP 32):** `TX2 = GPIO17 (G17)`, `RX2 = GPIO16 (G16)`.  
**Salida:** no genera TXT; solo **eco+1**
- **Validaciones rápidas:** mantener el **eco inmediato** (sin `delay` largos); revisar **TX↔RX**, **GND** y el mismo **baud**.

### Código — `esp32_uart_slave.py`

{% raw %}
~~~c++
// ESP32_UART_ECHO_PLUS_ONE_FIXEDLEN.ino
#define ESP32_RX_PIN 16
#define ESP32_TX_PIN 17
const long DUT_BAUD = 38400;

char buf[16];
int  idx = 0;

void setup() {
  Serial.begin(115200);
  Serial2.begin(DUT_BAUD, SERIAL_8N1, ESP32_RX_PIN, ESP32_TX_PIN);
  Serial.println("# ESP32 echo+1 (4 digitos fijos) listo");
}

void loop() {
  while (Serial2.available()) {
    char c = Serial2.read();
    if (c == '\r') continue;
    if (c != '\n') {
      if (idx < (int)sizeof(buf)-1) buf[idx++] = c;
    } else {
      buf[idx] = '\0';
      idx = 0;
      int v = atoi(buf) + 1;

      char out[16];
      int n = snprintf(out, sizeof(out), "%04u\n", (unsigned)v);
      Serial2.write((uint8_t*)out, n);
    }
  }
}
~~~
{% endraw %}
