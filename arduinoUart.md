---
title: "Arduino Nano"
parent: "Códigos"
nav_order: 1
layout: default
has_toc: true
---

## Rol y formato

- **Maestro:** inicia ping, mide **RTT** con `micros()` y emite CSV por **Serial** como `index,rtt_us` (1000 iteraciones).
- **Esclavo:** responde **eco+1** inmediatamente al recibir `"%04d\n"`.

**Baud / trama:** `38400`, `8N1`, `"%04d\n"`.

**Pines (SoftwareSerial):** `RX = D10` (desde TX del otro), `TX = D11` (hacia RX del otro).

**Niveles lógicos:** Nano es **5 V**; si el otro dispositivo es **3.3 V** (RP2040/ESP32), usa **conversor de nivel** o **divisores**.

**Cableado:** `TX ↔ RX` cruzado y **GND común**.

**Uso (maestro):** abrir **Monitor Serie** @ `115200`, copiar el **CSV** para graficar.

**Uso (esclavo):** cargar y dejar responder (no imprime CSV).

### Maestro — `nano_uart_master_rtt.ino`

- **Placa:** Arduino Nano (ATmega328P)  
- **Baud/trama:** 38400, 8N1  
- **Pines (SoftwareSerial):** RX=D10, TX=D11  
- **Salida:** CSV por Serial (`index,rtt_us`) con 1000 iteraciones

```cpp
// nano_uart_master_rtt.ino  — Arduino Nano (ATmega328P)
// Mide RTT (round-trip) enviando 1000 mensajes "%04d\n" y leyendo eco+1.
// Imprime CSV por Serial: "index,rtt_us"

#include <SoftwareSerial.h>

const long DUT_BAUD = 38400;                 // debe coincidir con el esclavo
const uint8_t RX_PIN = 10;                   // D10 <- TX del otro
const uint8_t TX_PIN = 11;                   // D11 -> RX del otro
SoftwareSerial DUT(RX_PIN, TX_PIN);          // RX, TX

const uint16_t N_ITER = 1000;
const unsigned long TIMEOUT_MS = 1000;

bool readLine(Stream& s, char* out, size_t maxlen, unsigned long timeout_ms) {
  unsigned long t0 = millis();
  size_t i = 0;
  while (millis() - t0 < timeout_ms) {
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

  Serial.println(F("# Test UART 4 digitos: Nano maestro -> eco+1"));
  Serial.print(F("# BAUD ")); Serial.println(DUT_BAUD);
  Serial.println(F("# CSV: index,rtt_us"));
}

void loop() {
  // Estadística online (Welford)
  double mean = 0.0, M2 = 0.0; uint32_t valid = 0;

  char rxbuf[16];
  int value = 1;

  for (uint16_t idx = 1; idx <= N_ITER; idx++) {
    char msg[8];
    int len = snprintf(msg, sizeof(msg), "%04u\n", (unsigned)value);

    unsigned long t0 = micros();
    DUT.write((uint8_t*)msg, len);

    if (!readLine(DUT, rxbuf, sizeof(rxbuf), TIMEOUT_MS)) {
      Serial.print(idx); Serial.println(F(",TIMEOUT"));
      continue;
    }

    unsigned long t1 = micros();
    unsigned long rtt = (unsigned long)(t1 - t0);

    Serial.print(idx); Serial.print(","); Serial.println(rtt);

    // stats
    valid++;
    double x = (double)rtt;
    double d = x - mean; mean += d / valid; M2 += d * (x - mean);

    value = atoi(rxbuf); // el esclavo responde value+1
  }

  Serial.println(F("# DONE"));
  if (valid >= 2) {
    double var = M2 / (valid - 1);
    double sd  = sqrt(var);
    Serial.print(F("# VALID,"));  Serial.println(valid);
    Serial.print(F("# AVG_US,")); Serial.println(mean, 3);
    Serial.print(F("# STDDEV_US,")); Serial.println(sd, 3);
  } else {
    Serial.println(F("# VALID,0"));
  }

  while (1) { delay(1000); }  // detener
}
