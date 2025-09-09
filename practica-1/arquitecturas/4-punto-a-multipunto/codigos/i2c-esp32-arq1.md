---
title: "I2C — ESP32"
parent: "Firmware"
grand_parent: "Arq.1 — Punto a Multipunto"
layout: default
nav_order: 4
---

# Dispositivo A

{% raw %}
~~~c++
#include <Arduino.h>
#include <Wire.h>

constexpr uint8_t  B_ADDR = 0x12;
constexpr uint8_t  C_ADDR = 0x13;

constexpr int SDA_PIN = 21;
constexpr int SCL_PIN = 22;

constexpr int BTN_B = 18;   // a GND
constexpr int BTN_C = 19;   // a GND

constexpr uint32_t I2C_HZ       = 100000;
constexpr uint32_t DEBOUNCE_MS  = 30;
constexpr uint32_t HEARTBEAT_MS = 1000;

static inline void sendVal(uint8_t addr, bool v) {
  Wire.beginTransmission(addr);
  Wire.write(v ? '1' : '0');   // 1 byte
  Wire.endTransmission(true);
}

void setup() {
  Serial.begin(115200);
  pinMode(BTN_B, INPUT_PULLUP);
  pinMode(BTN_C, INPUT_PULLUP);
  Wire.begin(SDA_PIN, SCL_PIN);
  Wire.setClock(I2C_HZ);
  Serial.println(F("# A master listo"));
}

void loop() {
  static int lastB=-1, lastC=-1;
  static uint32_t tB=0, tC=0, lastBeat=0;
  static bool stateB=false, stateC=false;

  const uint32_t now = millis();
  const int b = (digitalRead(BTN_B)==LOW)?1:0;
  const int c = (digitalRead(BTN_C)==LOW)?1:0;

  if (b!=lastB && (now-tB)>DEBOUNCE_MS) {
    tB=now; lastB=b; stateB=(b==1);
    sendVal(B_ADDR, stateB);
    Serial.printf("B <- %d\n", stateB);
  }
  if (c!=lastC && (now-tC)>DEBOUNCE_MS) {
    tC=now; lastC=c; stateC=(c==1);
    sendVal(C_ADDR, stateC);
    Serial.printf("C <- %d\n", stateC);
  }
  if (now-lastBeat>=HEARTBEAT_MS) {      // re-sincroniza
    lastBeat=now;
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

constexpr uint8_t  SLAVE_ADDR = 0x12;
constexpr int SDA_PIN = 21, SCL_PIN = 22;
constexpr int LED_PIN = 2;
constexpr uint32_t I2C_HZ = 100000;

volatile bool pending=false;
volatile uint8_t lastByte=0;

void onReceiveHandler(int n) {
  if (n>=1) { lastByte = Wire.read(); while (Wire.available()) (void)Wire.read(); pending=true; }
}

void setup() {
  pinMode(LED_PIN, OUTPUT); digitalWrite(LED_PIN, LOW);
  Serial.begin(115200);
  Wire.begin((uint8_t)SLAVE_ADDR, SDA_PIN, SCL_PIN, I2C_HZ);
  Wire.onReceive(onReceiveHandler);
  Serial.println(F("# B slave listo (0x12)"));
}

void loop() {
  if (pending) {
    noInterrupts(); uint8_t b=lastByte; pending=false; interrupts();
    bool on = (b=='1' || b==1);
    digitalWrite(LED_PIN, on?HIGH:LOW);
  }
}
~~~
{% endraw %}

# Dispositivo C

{% raw %}
~~~c++
#include <Arduino.h>
#include <Wire.h>

constexpr uint8_t  SLAVE_ADDR = 0x13;
constexpr int SDA_PIN = 21, SCL_PIN = 22;
constexpr int LED_PIN = 4;
constexpr uint32_t I2C_HZ = 100000;

volatile bool pending=false;
volatile uint8_t lastByte=0;

void onReceiveHandler(int n) {
  if (n>=1) { lastByte = Wire.read(); while (Wire.available()) (void)Wire.read(); pending=true; }
}

void setup() {
  pinMode(LED_PIN, OUTPUT); digitalWrite(LED_PIN, LOW);
  Serial.begin(115200);
  Wire.begin((uint8_t)SLAVE_ADDR, SDA_PIN, SCL_PIN, I2C_HZ);
  Wire.onReceive(onReceiveHandler);
  Serial.println(F("# C slave listo (0x13)"));
}

void loop() {
  if (pending) {
    noInterrupts(); uint8_t b=lastByte; pending=false; interrupts();
    bool on = (b=='1' || b==1);
    digitalWrite(LED_PIN, on?HIGH:LOW);
  }
}
~~~
{% endraw %}