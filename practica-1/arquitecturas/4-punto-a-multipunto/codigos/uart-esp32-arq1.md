---
title: "UART — ESP32"
parent: "Firmware"
grand_parent: "Arq.1 — Punto a Multipunto"
layout: default
nav_order: 2
---

# Dispositivo A

{% raw %}
~~~c++
#include <Arduino.h>

// ===== Config =====
constexpr uint32_t BAUD         = 115200;
constexpr uint32_t DEBOUNCE_MS  = 30;
constexpr uint32_t HEARTBEAT_MS = 1000;

// ===== Pines (lado derecho) =====
constexpr int BTN_B = 18;         // a GND, INPUT_PULLUP
constexpr int BTN_C = 19;         // a GND, INPUT_PULLUP

// UART hacia Slave B
HardwareSerial UART_B(1);
constexpr int TX_B = 17;          // Master TX -> Slave-B RX(16)

// UART hacia Slave C
HardwareSerial UART_C(2);
constexpr int TX_C = 23;          // Master TX -> Slave-C RX(22)

// ===== Helpers =====
inline void sendBool(HardwareSerial& port, bool v) {
  port.write(v ? '1' : '0');
  port.write('\n');
}

void setup() {
  Serial.begin(115200);

  pinMode(BTN_B, INPUT_PULLUP);
  pinMode(BTN_C, INPUT_PULLUP);

  // Sólo TX (RX = -1) para enlaces unidireccionales
  UART_B.begin(BAUD, SERIAL_8N1, -1, TX_B);
  UART_C.begin(BAUD, SERIAL_8N1, -1, TX_C);

  Serial.println(F("# Master listo (pines derechos)"));
}

void loop() {
  static int lastB = -1, lastC = -1;
  static uint32_t tB = 0, tC = 0;
  static bool stateB = false, stateC = false;
  static uint32_t lastBeat = 0;

  const uint32_t now = millis();
  const int b = (digitalRead(BTN_B) == LOW) ? 1 : 0;  // activo a LOW
  const int c = (digitalRead(BTN_C) == LOW) ? 1 : 0;

  if (b != lastB && (now - tB) > DEBOUNCE_MS) {
    tB = now; lastB = b;
    stateB = (b == 1);
    sendBool(UART_B, stateB);
    Serial.printf("B -> %d\n", stateB);
  }

  if (c != lastC && (now - tC) > DEBOUNCE_MS) {
    tC = now; lastC = c;
    stateC = (c == 1);
    sendBool(UART_C, stateC);
    Serial.printf("C -> %d\n", stateC);
  }

  // Heartbeat para re-sincronizar
  if (now - lastBeat >= HEARTBEAT_MS) {
    lastBeat = now;
    sendBool(UART_B, stateB);
    sendBool(UART_C, stateC);
  }
}
~~~
{% endraw %}

# Dispositivo B y C

{% raw %}
~~~c++
#include <Arduino.h>

constexpr uint32_t BAUD         = 115200;
constexpr int      LED_PIN      = 2;      // lado derecho
constexpr int      RX_PIN       = 16;     // llega desde Master TX_B (17)
constexpr uint32_t RX_TIMEOUTMS = 5000;   // watchdog

HardwareSerial UART_RX(2);  // puerto propio del slave
uint32_t lastRx = 0;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);

  UART_RX.begin(BAUD, SERIAL_8N1, RX_PIN, -1);  // sólo RX
  Serial.begin(115200);
  Serial.println(F("# Slave B listo (pines derechos)"));
  lastRx = millis();
}

void loop() {
  while (UART_RX.available()) {
    char ch = (char)UART_RX.read();
    if (ch == '0' || ch == '1') {
      digitalWrite(LED_PIN, ch == '1' ? HIGH : LOW);
      lastRx = millis();
    }
    // ignorar CR/LF/otros
  }
  if (millis() - lastRx > RX_TIMEOUTMS) {
    digitalWrite(LED_PIN, LOW);
  }
}
~~~
{% endraw %}