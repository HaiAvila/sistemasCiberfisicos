---
title: "Arduino Nano — Maestro"
parent: "codigos"
grand_parent: "UART"
layout: default
nav_order: 1
render_with_liquid: false
---

# Arduino Nano — Maestro

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

```cpp
// NANO_INIT_UART_RTT_FIXED.ino
#include <SoftwareSerial.h>

const uint8_t RX_PIN = 10;   // D10  <- ESP32 TX2 (GPIO17)
const uint8_t TX_PIN = 11;   // D11  -> divisor -> ESP32 RX2 (GPIO16)
SoftwareSerial DUT(RX_PIN, TX_PIN); // RX, TX

const long   DUT_BAUD       = 38400;
const uint16_t N_ITER       = 1000;
const unsigned long TIMEOUT_MS = 1000;

char rxbuf[16];

void flushDUT() { while (DUT.available()) DUT.read(); }

bool readLine(Stream& s, char* out, size_t maxlen) {
  unsigned long t0 = millis();
  size_t i = 0;
  while (millis() - t0 < TIMEOUT_MS) {
    if (s.available()) {
      char c = s.read();
      if (c == '\r') continue;
      if (c == '\n') { out[i] = '\0'; return true; }
      if (i < maxlen - 1) out[i++] = c;
    }
  }
  out[0] = '\0';
  return false;
}

void setup() {
  Serial.begin(115200);
  DUT.begin(DUT_BAUD);
  delay(200);
  Serial.println(F("# Test UART fijo 4 digitos: Nano iniciador -> ESP32 eco+1"));
  Serial.print(F("# BAUD ")); Serial.println(DUT_BAUD);
  Serial.println(F("# CSV: index,rtt_us"));
  flushDUT();
}

void loop() {
  uint16_t idx = 0;
  int value = 1; // primer envío
  for (idx = 1; idx <= N_ITER; idx++) {
    // SIEMPRE 4 dígitos + '\n' -> 5 chars fijos (ej: "0001\n")
    char msg[8];
    int len = snprintf(msg, sizeof(msg), "%04u\n", (unsigned)value);

    unsigned long t0 = micros();
    DUT.write((uint8_t*)msg, len);

    if (!readLine(DUT, rxbuf, sizeof(rxbuf))) {
      Serial.print(idx); Serial.println(F(",TIMEOUT"));
      continue;
    }
    unsigned long t1 = micros();
    unsigned long rtt = (unsigned long)(t1 - t0);

    int resp = atoi(rxbuf); // debería ser value+1 en ascii fijo

    Serial.print(idx);
    Serial.print(",");
    Serial.println(rtt);

    value = resp; // siguiente envío es lo que devolvió el ESP32
  }

  Serial.println(F("# DONE"));
  while (1) { delay(1000); }
}
