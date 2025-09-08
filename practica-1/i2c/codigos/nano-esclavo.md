---
title: "Arduino Nano — Esclavo"
parent: "Firmware"
grand_parent: "I2C"
layout: default
nav_order: 2
---

# Arduino Nano — Esclavo

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
~~~c++
#include <Wire.h>

#define I2C_SLAVE_ADDR 0x08  // Dirección I2C del Arduino

void setup() {
  Wire.begin(I2C_SLAVE_ADDR);
  Wire.onRequest(requestEvent);  // Qué hacer cuando el master pide datos
  Serial.begin(9600);
}

void loop() {
  // Aquí podrías actualizar un valor de sensor, etc.
  delay(100);
}

void requestEvent() {
  static int counter = 0;
  counter++;
  Wire.write(counter);  // Enviar un valor de 0–255 al master
  Serial.print("Enviado: ");
  Serial.println(counter);
}
~~~
{% endraw %}