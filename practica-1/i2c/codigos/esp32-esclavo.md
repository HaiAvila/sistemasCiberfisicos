---
title: "ESP32 — Esclavo"
parent: "Firmware"
grand_parent: "I2C"
layout: default
nav_order: 6
---

# ESP32 — Esclavo

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
```cpp

// NANO_I2C_MASTER_RTT_1000.ino
// Master I2C (Nano/ATmega328P) -> ESP32 slave (0x12).
// Envía "hola_0001"... y mide RTT (write+read). Imprime formato para el logger.

#if !defined(__AVR_ATmega328P__)
  #error "Selecciona 'Arduino Nano/Uno (ATmega328P)' en Tools->Board."
#endif

#include <Wire.h>

constexpr uint8_t  SLAVE_ADDR   = 0x12;
constexpr uint32_t I2C_CLK_HZ   = 100000;   // 100 kHz (puedes subir a 400 kHz)
constexpr uint8_t  MAX_PAYLOAD  = 7;       // mantener compatibilidad con AVR
constexpr uint16_t N_SAMPLES    = 1000;
// Pequeña pausa opcional entre transacciones; ajusta si hace falta estabilidad
constexpr uint16_t GAP_US       = 2000;

bool i2c_exchange(const char* tx, char* rx, uint8_t rx_cap) {
  // 1) WRITE [LEN][PAYLOAD]
  uint8_t len = 0; while (tx[len] && len < MAX_PAYLOAD) len++;

  Wire.beginTransmission(SLAVE_ADDR);
  Wire.write(len);
  if (len) Wire.write((const uint8_t*)tx, len);
  uint8_t rc = Wire.endTransmission(true); // STOP
  if (rc != 0) return false;

  // 2) READ [LEN][PAYLOAD]
  int n = Wire.requestFrom((int)SLAVE_ADDR, 32, (int)true);
  if (n < 1) return false;

  uint8_t rlen = Wire.read();
  if (rlen > MAX_PAYLOAD) rlen = MAX_PAYLOAD;
  if (rlen > (uint8_t)(n - 1)) rlen = (n > 1) ? (uint8_t)(n - 1) : 0;

  uint8_t i = 0;
  while (Wire.available() && i < rlen && i < (rx_cap - 1)) rx[i++] = (char)Wire.read();
  while (Wire.available()) (void)Wire.read(); // drenar extras
  rx[i] = '\0';

  return true;
}

void setup() {
  Serial.begin(115200);
  // Importante: NO activar INPUT_PULLUP en A4/A5 (queremos 3.3 V en el bus)
  pinMode(A4, INPUT);
  pinMode(A5, INPUT);

  Wire.begin();                 // Nano master en A4/A5
  #if ARDUINO >= 10605
    Wire.setClock(I2C_CLK_HZ);
  #endif

  // Cabecera para tu logger Python
  Serial.print(F("# BAUD "));
  Serial.println(I2C_CLK_HZ);
}

void loop() {
  static uint16_t k = 1;
  if (k > N_SAMPLES) {
    Serial.println(F("# DONE"));
    while (1) ; // detener
  }

  // Mensaje a eco: "hola_####"
  char tx[32];
  snprintf(tx, sizeof(tx), "hola_%04u", k);

  unsigned long t0 = micros();
  char rx[32];
  bool ok = i2c_exchange(tx, rx, sizeof(rx));
  unsigned long dt = micros() - t0;

  if (!ok) {
    Serial.print(k); Serial.println(F(",TIMEOUT"));
  } else {
    // (Opcional) validar eco: if (strcmp(tx, rx) != 0) { ... }
    Serial.print(k); Serial.print(F(",")); Serial.println((unsigned long)dt);
  }

  k++;
  //delayMicroseconds(GAP_US);
}


{% endraw %}