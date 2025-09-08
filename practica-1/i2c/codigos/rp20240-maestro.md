---
title: "RP2040 — Maestro"
parent: "codigos"
grand_parent: "I2C"
layout: default
nav_order: 3
---

# RP2040 — Maestro

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~c++
#include <Wire.h>

#define SLAVE_ADDR 0x10  // Dirección I2C del RP2040
#define N_ITER 1000

void setup() {
  Serial.begin(115200);
  Wire.begin(); // Arduino como maestro
  delay(500);
  Serial.println("# I2C Latency Test: Arduino → RP2040");

  for (int i = 1; i <= N_ITER; i++) {
    uint16_t value = i;
    unsigned long t0 = micros();

    Wire.beginTransmission(SLAVE_ADDR);
    Wire.write((uint8_t*)&value, 2);
    Wire.endTransmission();

    delayMicroseconds(100);  // Pequeña pausa para dar tiempo al esclavo

    Wire.requestFrom(SLAVE_ADDR, 2);
    if (Wire.available() < 2) {
      Serial.print(i); Serial.println(",TIMEOUT");
      continue;
    }

    uint16_t response = Wire.read() | (Wire.read() << 8);
    unsigned long t1 = micros();
    unsigned long rtt = t1 - t0;

    Serial.print(i);
    Serial.print(",");
    Serial.println(rtt);
  }

  Serial.println("# DONE");
}
void loop() {}
~~~
{% endraw %}