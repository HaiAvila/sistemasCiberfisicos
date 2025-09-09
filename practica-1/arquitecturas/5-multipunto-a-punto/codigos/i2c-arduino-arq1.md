---
title: "I2C — Arduino"
parent: "Firmware"
grand_parent: "Arq.2 — Multipunto a Punto"
layout: default
nav_order: 3
---

# Dispositivo A

{% raw %}
~~~c++

~~~
{% endraw %}

# Dispositivo B

{% raw %}
~~~c++
#include <Arduino.h>
#include <Wire.h>
#include <util/atomic.h>

constexpr uint8_t SLAVE_ADDR = 0x12;
constexpr uint8_t LED_PIN    = LED_BUILTIN;
constexpr uint32_t I2C_HZ    = 100000;

volatile bool     pending = false;
volatile uint8_t  lastByte = 0;

void onReceiveHandler(int n) {
  if (n >= 1) {
    lastByte = Wire.read();                 // leemos 1er byte (0/'0' o 1/'1')
    while (Wire.available()) (void)Wire.read(); // drena extras si llegaran
    pending = true;
  } else {
    while (Wire.available()) (void)Wire.read();
  }
}

void setup() {
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);

  Wire.begin(SLAVE_ADDR);                   // Slave en 0x12 (A4/A5)
  #if ARDUINO >= 10605
    Wire.setClock(I2C_HZ);
  #endif
  Wire.onReceive(onReceiveHandler);
}

void loop() {
  if (pending) {
    uint8_t b;
    ATOMIC_BLOCK(ATOMIC_RESTORESTATE) { b = lastByte; pending = false; }
    bool on = (b == 1 || b == '1');
    digitalWrite(LED_PIN, on ? HIGH : LOW);
  }
}
~~~
{% endraw %}

# Dispositivo c

{% raw %}
~~~c++

~~~
{% endraw %}