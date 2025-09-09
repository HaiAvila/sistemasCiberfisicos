---
title: "UART — ESP32"
parent: "Firmware"
grand_parent: "Arq.2 — Multipunto a Punto"
layout: default
nav_order: 2
---

# Dispositivo A

{% raw %}
~~~c++
#include <Arduino.h>

// ----- Config -----
constexpr uint32_t BAUD          = 115200;
constexpr uint32_t RX_WATCHDOGMS = 0;      // 0 = desactivado; p. ej. 5000 apaga LED si no hay tráfico

// ----- Pines (lado derecho) -----
// RX desde B y C:
constexpr int RX_B = 16;   // viene de TX=17 en B
constexpr int RX_C = 22;   // viene de TX=23 en C

// LEDs:
constexpr int LED1 = 21;   // controlado por B
constexpr int LED2 = 4;    // controlado por C

// Dos UARTs dedicados (solo RX):
HardwareSerial UartB(1);
HardwareSerial UartC(2);

// Buffers de línea
char lineB[16]; uint8_t idxB = 0;
char lineC[16]; uint8_t idxC = 0;

uint32_t lastRxB = 0, lastRxC = 0;

inline void applyCmd(int ledPin, char ch) {
  if (ch == '0' || ch == '1') {
    digitalWrite(ledPin, ch == '1' ? HIGH : LOW);
  }
}

inline void readPort(HardwareSerial& port, char* buf, uint8_t& idx, int ledPin, uint32_t& lastTs) {
  while (port.available()) {
    char c = (char)port.read();
    if (c == '\n' || c == '\r') {
      if (idx > 0) {
        buf[idx] = '\0';
        applyCmd(ledPin, buf[0]); // esperamos "0" o "1"
        idx = 0;
        lastTs = millis();
      }
    } else if (idx < 15) {
      buf[idx++] = c;
    } else {
      idx = 0; // overflow -> resetea
    }
  }
}

void setup() {
  pinMode(LED1, OUTPUT); digitalWrite(LED1, LOW);
  pinMode(LED2, OUTPUT); digitalWrite(LED2, LOW);

  Serial.begin(115200);

  // Solo RX (TX=-1)
  UartB.begin(BAUD, SERIAL_8N1, RX_B, -1);
  UartC.begin(BAUD, SERIAL_8N1, RX_C, -1);

  Serial.println(F("# A listo: dual RX (B->LED1, C->LED2)"));
  lastRxB = lastRxC = millis();
}

void loop() {
  readPort(UartB, lineB, idxB, LED1, lastRxB);
  readPort(UartC, lineC, idxC, LED2, lastRxC);

  if (RX_WATCHDOGMS) {
    uint32_t now = millis();
    if (now - lastRxB > RX_WATCHDOGMS) digitalWrite(LED1, LOW);
    if (now - lastRxC > RX_WATCHDOGMS) digitalWrite(LED2, LOW);
  }
}
~~~
{% endraw %}

# Dispositivo B y C

{% raw %}
~~~c++
#include <Arduino.h>

constexpr uint32_t BAUD         = 115200;
constexpr uint32_t DEBOUNCE_MS  = 30;
constexpr uint32_t HEARTBEAT_MS = 1000;

// Pines (lado derecho)
constexpr int BTN_PIN = 18;  // a GND, INPUT_PULLUP
constexpr int TX_PIN  = 17;  // va a RX_B (16) en A

HardwareSerial Uart(1);

inline void sendBool(bool v) {
  Uart.write(v ? '1' : '0');
  Uart.write('\n');
}

void setup() {
  Serial.begin(115200);
  pinMode(BTN_PIN, INPUT_PULLUP);

  // solo TX (RX=-1)
  Uart.begin(BAUD, SERIAL_8N1, -1, TX_PIN);
  Serial.println(F("# B listo (btn->TX)"));
}

void loop() {
  static int last = -1;
  static uint32_t tLast = 0;
  static bool state = false;
  static uint32_t lastBeat = 0;

  uint32_t now = millis();
  int b = (digitalRead(BTN_PIN) == LOW) ? 1 : 0;

  if (b != last && (now - tLast) > DEBOUNCE_MS) {
    tLast = now; last = b;
    state = (b == 1);
    sendBool(state);
    Serial.printf("B send: %d\n", state);
  }

  if (now - lastBeat >= HEARTBEAT_MS) {
    lastBeat = now;
    sendBool(state); // keep-alive
  }
}
~~~
{% endraw %}