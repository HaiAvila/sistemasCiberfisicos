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

**Pines (UART0 en ESP 32):** `TX2 = GPIO17 (G17)`, `RX2 = GPIO16 (G16)`.  

**Niveles lógicos:** ESP32 es **3.3 V**; el otro dispositivo es **3.3 V** (RP 2040).  

**Cableado:** `TX ↔ RX` cruzado y **GND común**.  

**Uso (maestro):** en **Thonny**, guardar y ejecutar `esp32_master_uart.py`; al terminar, descargar el TXT del dispositivo.  
**Uso (esclavo):** guardar como `main.py` (o ejecutar manualmente) y dejar corriendo mientras el maestro mide.

### Maestro — `rp2040_uart_master.py`
**Plataforma:** Seeed XIAO RP2040 (MicroPython)  
**Baud/trama:** `38400`, `8N1`  
**Pines (UART0):** `TX = P0 (D6)`, `RX = P1 (D7)`  
**Salida:** CSV en el dispositivo (`index,rtt_us`) con **1000** iteraciones
- **Validaciones rápidas:** si ves `TIMEOUT`, revisa **TX↔RX**, **GND**, mismo **baud** en ambos; evita cargas de CPU en el esclavo.

{% raw %}
~~~c++
# esp32_uart_master.py  (ESP32 - C++)
#include <HardwareSerial.h>

// Usamos UART2 -> pines recomendados RX=16, TX=17
HardwareSerial SerialPort(2); 

void setup() {
  // Monitor por USB
  Serial.begin(9600);

  // UART2 para comunicación con la RP2040
  SerialPort.begin(9600, SERIAL_8N1, 16, 17);  
  Serial.println("ESP32 listo para recibir datos...");
}

void loop() {
  // Si hay datos desde la RP2040
  if (SerialPort.available()) {
    String recibido = SerialPort.readStringUntil('\n'); // lee hasta salto de línea
    
    if (recibido.length() > 0) {
      int numero = recibido.toInt();   // convierte a entero
      int respuesta = numero + 1;      // suma 1

      // Formatear la respuesta en 4 dígitos
      char buffer[6];
      sprintf(buffer, "%04d\n", respuesta);

      // Enviar de regreso a la RP2040
      SerialPort.print(buffer);

      // Mostrar en monitor USB para depuración
      Serial.print("Recibido: ");
      Serial.print(recibido);
      Serial.print(" -> Enviado: ");
      Serial.println(buffer);
    }
  }
}

~~~
{% endraw %}

### Esclavo — `rp2040_uart_slave.py`
**Plataforma:** Seeed XIAO RP2040 (MicroPython)  
**Baud/trama:** `38400`, `8N1`  
**Pines (UART0):** `TX = P0 (D6)`, `RX = P1 (D7)`  
**Salida:** no genera CSV; solo **eco+1**
- **Validaciones rápidas:** mantener el **eco inmediato** (sin `delay` largos); revisar **TX↔RX**, **GND** y el mismo **baud**.

### Código — `rp2040_uart_slave.py`

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
