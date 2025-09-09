---
title: "I2C — Arduino"
parent: "Firmware"
grand_parent: "Arq.1 — Punto a Multipunto"
layout: default
nav_order: 3
---

# Dispositivo A

{% raw %}
~~~c++
#include <Arduino.h>
#include <Wire.h>

constexpr uint8_t  B_ADDR = 0x12;
constexpr uint8_t  C_ADDR = 0x13;

constexpr uint8_t  BTN_B  = 2;   // a GND (INPUT_PULLUP)
constexpr uint8_t  BTN_C  = 3;   // a GND (INPUT_PULLUP)

constexpr uint32_t I2C_HZ       = 100000;   // 100 kHz
constexpr uint32_t DEBOUNCE_MS  = 30;
constexpr uint32_t HEARTBEAT_MS = 1000;

static inline void sendVal(uint8_t addr, bool v) {
  Wire.beginTransmission(addr);
  Wire.write(v ? '1' : '0');    // 1 byte
  (void)Wire.endTransmission(true);
}

void setup() {
  pinMode(BTN_B, INPUT_PULLUP);
  pinMode(BTN_C, INPUT_PULLUP);

  Wire.begin();                 // A como master (A4/A5)
  #if ARDUINO >= 10605
    Wire.setClock(I2C_HZ);
  #endif
}

void loop() {
  static int lastB=-1, lastC=-1;
  static uint32_t tB=0, tC=0, lastBeat=0;
  static bool stateB=false, stateC=false;

  const uint32_t now = millis();
  const int b = (digitalRead(BTN_B)==LOW) ? 1 : 0;
  const int c = (digitalRead(BTN_C)==LOW) ? 1 : 0;

  if (b!=lastB && (now-tB)>DEBOUNCE_MS) {
    tB=now; lastB=b; stateB=(b==1);
    sendVal(B_ADDR, stateB);
  }
  if (c!=lastC && (now-tC)>DEBOUNCE_MS) {
    tC=now; lastC=c; stateC=(c==1);
    sendVal(C_ADDR, stateC);
  }

  if (now-lastBeat >= HEARTBEAT_MS) {      // re-sincroniza por si se perdió algo
    lastBeat = now;
    sendVal(B_ADDR, stateB);
    sendVal(C_ADDR, stateC);
  }
}
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

# Dispositivo C

{% raw %}
~~~c++
#include <Arduino.h>
#include <Wire.h>
#include <util/atomic.h>

constexpr uint8_t SLAVE_ADDR = 0x13;
constexpr uint8_t LED_PIN    = LED_BUILTIN;
constexpr uint32_t I2C_HZ    = 100000;

volatile bool     pending = false;
volatile uint8_t  lastByte = 0;

void onReceiveHandler(int n) {
  if (n >= 1) {
    lastByte = Wire.read();
    while (Wire.available()) (void)Wire.read();
    pending = true;
  } else {
    while (Wire.available()) (void)Wire.read();
  }
}

void setup() {
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);

  Wire.begin(SLAVE_ADDR);   // Slave en 0x13 (A4/A5)
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