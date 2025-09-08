---
title: "RP2040 — Esclavo"
parent: "codigos"
grand_parent: "I2C"
layout: default
nav_order: 1
---

# RP2040 — Esclavo

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~c++
// XIAO RP2040 - I2C SLAVE echo+1 en SDA/SCL (P6/P7) usando Wire1
#include <Wire.h>

constexpr uint8_t I2C_ADDR = 0x42;
char inbuf[8]; int idx = 0;
char outbuf[8] = "0001\n";

// --- usa Wire1 en TODO ---
void onReceiveI2C(int nbytes) {
  while (Wire1.available()) {
    char c = Wire1.read();
    if (c == '\r') continue;
    if (c != '\n') {
      if (idx < (int)sizeof(inbuf) - 1) inbuf[idx++] = c;
    } else {
      inbuf[idx] = '\0'; idx = 0;
      int v = atoi(inbuf) + 1;
      snprintf(outbuf, sizeof(outbuf), "%04u\n", (unsigned)v);
    }
  }
}

void onRequestI2C() {
  Wire1.write((const uint8_t*)outbuf, 5); // 4 dígitos + '\n'
}

void setup() {
  Serial.begin(115200);
  // Estos macros apuntan a los pines etiquetados SDA/SCL de la placa
  Wire1.setSDA(SDA);
  Wire1.setSCL(SCL);
  Wire1.begin(I2C_ADDR);          // esclavo en 0x42 sobre Wire1 (SDA/SCL)
  Wire1.onReceive(onReceiveI2C);
  Wire1.onRequest(onRequestI2C);
  Serial.println(F("# XIAO RP2040 slave 0x42 en SDA/SCL listo"));
}

void loop() { /* nada */ }
~~~
{% endraw %}