---
title: "UART — Arduino"
parent: "Firmware"
grand_parent: "Arq.1 — Punto a Multipunto"
layout: default
nav_order: 1
---

# Dispositivo A

{% raw %}
~~~c++
#include <Arduino.h>
#include <SoftwareSerial.h>

constexpr unsigned long BAUD         = 57600;
constexpr byte BTN_B = 2;      // a GND
constexpr byte BTN_C = 3;      // a GND
constexpr unsigned long DEBOUNCE_MS  = 30;
constexpr unsigned long HEARTBEAT_MS = 1000;

// UART_B: RX=8 (sin usar), TX=9 -> B.RX(D0)
SoftwareSerial UART_B(8, 9);   // (RX, TX)  RX no se usa
// UART_C: RX=6 (sin usar), TX=5 -> C.RX(D0)
SoftwareSerial UART_C(6, 5);   // (RX, TX)  RX no se usa

inline void sendCmd(SoftwareSerial &port, char id, bool val) {
  port.print(id);
  port.print(',');
  port.print(val ? '1' : '0');
  port.print('\n');
}

void setup() {
  pinMode(BTN_B, INPUT_PULLUP);
  pinMode(BTN_C, INPUT_PULLUP);

  UART_B.begin(BAUD);
  UART_C.begin(BAUD);
}

void loop() {
  static int lastB = -1, lastC = -1;
  static unsigned long tB = 0, tC = 0, lastBeat = 0;
  static bool stateB = false, stateC = false;

  const unsigned long now = millis();
  const int b = (digitalRead(BTN_B) == LOW) ? 1 : 0;
  const int c = (digitalRead(BTN_C) == LOW) ? 1 : 0;

  if (b != lastB && (now - tB) > DEBOUNCE_MS) {
    tB = now; lastB = b;
    stateB = (b == 1);
    sendCmd(UART_B, 'B', stateB);
  }
  if (c != lastC && (now - tC) > DEBOUNCE_MS) {
    tC = now; lastC = c;
    stateC = (c == 1);
    sendCmd(UART_C, 'C', stateC);
  }

  if (now - lastBeat >= HEARTBEAT_MS) {
    lastBeat = now;
    sendCmd(UART_B, 'B', stateB);  // keep-alive
    sendCmd(UART_C, 'C', stateC);
  }
}
~~~
{% endraw %}

# Dispositivo B y C

{% raw %}
~~~c++
#include <Arduino.h>

constexpr unsigned long BAUD = 57600;
constexpr char MY_ID = 'C';
constexpr byte LED_PIN = 12;

#define UART Serial

char line[24];
byte idx = 0;

void processLine() {
  line[idx] = '\0';
  if (line[0] == MY_ID && line[1] == ',' && (line[2] == '0' || line[2] == '1')) {
    digitalWrite(LED_PIN, (line[2] == '1') ? HIGH : LOW);
  }
  idx = 0;
}

void setup() {
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);
  UART.begin(BAUD);
}

void loop() {
  while (UART.available()) {
    char c = (char)UART.read();
    if (c == '\n' || c == '\r') {
      if (idx) processLine();
    } else if (idx < sizeof(line) - 1) {
      line[idx++] = c;
    } else {
      idx = 0;
    }
  }
}
~~~
{% endraw %}