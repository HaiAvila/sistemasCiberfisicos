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
