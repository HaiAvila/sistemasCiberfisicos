---
title: "Arduino Nano — Esclavo"
parent: "Firmware"
grand_parent: "UART"
layout: default
nav_order: 2
---

# Arduino Nano — Esclavo

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~cpp
// nano_uart_slave_echo.ino  — Arduino Nano
// Esclavo UART: lee "%04d\n", responde inmediatamente con (valor+1) en el mismo formato.

#include <SoftwareSerial.h>

const long DUT_BAUD = 38400;                 // debe coincidir con el maestro
const uint8_t RX_PIN = 10;                   // D10 <- TX del maestro
const uint8_t TX_PIN = 11;                   // D11 -> RX del maestro
SoftwareSerial DUT(RX_PIN, TX_PIN);          // RX, TX

char buf[16];
int idx = 0;

void setup() {
  DUT.begin(DUT_BAUD);
  // Opcional: Serial.begin(115200); Serial.println(F("# Esclavo listo"));
}

void loop() {
  while (DUT.available()) {
    char c = DUT.read();
    if (c == '\r') continue;
    if (c != '\n') {
      if (idx < (int)sizeof(buf) - 1) buf[idx++] = c;
    } else {
      buf[idx] = '\0'; idx = 0;

      int v = atoi(buf) + 1;
      char out[16];
      int n = snprintf(out, sizeof(out), "%04u\n", (unsigned)v);
      DUT.write((uint8_t*)out, n);
    }
  }
}
~~~
{% endraw %}