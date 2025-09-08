---
title: "Arduino Nano — Maestro"
parent: "Firmware"
grand_parent: "I2C"
layout: default
nav_order: 1
render_with_liquid: false
---

# Arduino Nano — Maestro

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

~~~c++
// ARDUINO_I2C_MASTER_RTT_CSV.ino
#include <Wire.h>

#define I2C_ADDR      0x42
const uint32_t I2C_HZ     = 100000;     // empieza en 100 kHz
const uint16_t N_ITER     = 1000;
const uint16_t RESP_LEN   = 5;          // "0002\n"
const uint32_t TIMEOUT_MS = 1000;

char rx[RESP_LEN + 1];

void setup() {
  Serial.begin(115200);
  Wire.begin();                 // maestro
  Wire.setClock(I2C_HZ);

  Serial.println(F("# Test I2C: Arduino maestro -> RP2040 esclavo echo+1"));
  Serial.print(F("# FREQ_HZ ")); Serial.println(I2C_HZ);
  Serial.println(F("# CSV: index,rtt_us"));
}

bool xfer_once(uint16_t value, uint32_t &rtt) {
  char msg[6];
  snprintf(msg, sizeof(msg), "%04u\n", (unsigned)value);

  unsigned long t0 = micros();

  // 1) WRITE (sin STOP -> repeated start)
  Wire.beginTransmission(I2C_ADDR);
  Wire.write((const uint8_t*)msg, 5);
  uint8_t err = Wire.endTransmission(false);   // false = no STOP
  if (err != 0) return false;

  // 2) READ (con STOP)
  Wire.requestFrom(I2C_ADDR, (int)RESP_LEN, (int)true);

  unsigned long tmax = millis() + TIMEOUT_MS;
  while (Wire.available() < RESP_LEN) {
    if ((long)(millis() - tmax) >= 0) return false;
  }

  for (int i = 0; i < RESP_LEN; i++) rx[i] = (char)Wire.read();
  rx[RESP_LEN] = '\0';

  unsigned long t2 = micros();
  rtt = t2 - t0;
  return true;
}

void loop() {
  double mean = 0.0, M2 = 0.0; uint32_t valid = 0;
  uint16_t value = 1;

  for (uint16_t idx = 1; idx <= N_ITER; idx++) {
    uint32_t rtt;
    if (!xfer_once(value, rtt)) { Serial.print(idx); Serial.println(F(",TIMEOUT")); continue; }
    Serial.print(idx); Serial.print(','); Serial.println(rtt);

    valid++; double x=rtt, d=x-mean; mean+=d/valid; M2+=d*(x-mean);

    value = (uint16_t)atoi(rx);  // debería ser value+1
  }

  Serial.println(F("# DONE"));
  if (valid >= 2) {
    double var = M2 / (valid - 1), sd = sqrt(var);
    Serial.print(F("# VALID,"));  Serial.println(valid);
    Serial.print(F("# AVG_US,")); Serial.println(mean, 3);
    Serial.print(F("# STDDEV_US,")); Serial.println(sd, 3);
  } else {
    Serial.println(F("# VALID,0"));
  }
  while (1) { delay(1000); }
}
~~~
{% endraw %}
