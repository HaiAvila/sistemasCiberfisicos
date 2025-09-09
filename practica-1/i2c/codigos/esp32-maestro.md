---
title: "ESP32 — Maestro"
parent: "Firmware"
grand_parent: "I2C"
layout: default
nav_order: 5
---

# ESP32 — Maestro

> Pega aquí tu sketch que **envía 1000 mensajes**, mide **RTT** y **imprime CSV** (`index,rtt_us`).

**Baud:** 38400 · **Trama:** `"%04d\n"` · **Pines sugeridos (SoftwareSerial):** RX=D10, TX=D11

{% raw %}
```cpp

// ESP32_I2C_INITIATOR_RTT.ino
// Envia uint16 (LSB,MSB) a un slave 0x12 y lee eco (v+1).
// Mide RTT con esp_timer_get_time() y loguea "index,rtt_us".

#if !defined(ARDUINO_ARCH_ESP32)
  #error "Este sketch es para ESP32. Selecciona una placa ESP32 en Tools->Board."
#endif

#include <Wire.h>
#include <esp_timer.h>

constexpr uint8_t I2C_SDA = 21;
constexpr uint8_t I2C_SCL = 22;
constexpr uint8_t SLAVE_ADDR = 0x12;

constexpr uint32_t I2C_CLK_HZ = 100000;   // 100 kHz (puedes probar 400000)
constexpr int N_ITER = 1000;
constexpr uint32_t READ_TIMEOUT_MS = 100; // timeout de lectura

bool i2c_exchange(uint16_t tx, uint16_t &rx){
  // Escribo 2 bytes (LSB,MSB), repeated start, y leo 2 bytes
  Wire.beginTransmission(SLAVE_ADDR);
  Wire.write((uint8_t)(tx & 0xFF));
  Wire.write((uint8_t)((tx >> 8) & 0xFF));
  int rc = Wire.endTransmission(false);   // false = repeated start
  if(rc != 0) return false;

  uint32_t t0 = millis();
  Wire.requestFrom((int)SLAVE_ADDR, 2);
  while(Wire.available() < 2){
    if(millis() - t0 > READ_TIMEOUT_MS) return false;
    delayMicroseconds(200);
  }
  uint8_t l = Wire.read();
  uint8_t h = Wire.read();
  rx = (uint16_t)(l | (h << 8));
  return true;
}

void setup(){
  Serial.begin(115200);
  Serial.println("# Test I2C: ESP32->Nano eco(+1)");
  Serial.print("# I2C_CLK "); Serial.println(I2C_CLK_HZ);

  Wire.begin(I2C_SDA, I2C_SCL);
  Wire.setClock(I2C_CLK_HZ);
}

void loop(){
  uint16_t value = 1;
  for(int idx=1; idx<=N_ITER; ++idx){
    int64_t t0 = esp_timer_get_time();     // us
    uint16_t resp = 0;
    bool ok = i2c_exchange(value, resp);
    int64_t t1 = esp_timer_get_time();
    uint32_t rtt = (uint32_t)(t1 - t0);

    if(!ok){
      Serial.print(idx); Serial.println(",TIMEOUT");
    }else{
      // (opcional) validar que resp == value+1
      Serial.print(idx); Serial.print(","); Serial.println(rtt);
      value = resp; // siguiente envío es lo recibido (+1)
    }
  }
  Serial.println("# DONE");
  while(1) delay(1000);
}


{% endraw %}