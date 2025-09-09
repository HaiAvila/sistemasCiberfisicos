---
title: "UART — Arduino"
parent: "Firmware"
grand_parent: "Arq.2 — Multipunto a Punto"
layout: default
nav_order: 1
---

# Dispositivo A

{% raw %}
~~~c++
#include <Arduino.h>
#include <AltSoftSerial.h>   // RX=8, TX=9 fijo en UNO/Nano

// Velocidades
constexpr unsigned long BAUD = 57600;

// LEDs de estado
constexpr byte LED_B = 2;    // comandos desde B (AltSoftSerial)
constexpr byte LED_C = 3;    // comandos desde C (HardwareSerial)

// UART de B (AltSoftSerial)
AltSoftSerial UART_B;         // RX=8 (conectar desde B.TX=D9), TX=9 (sin usar)

// UART de C = HardwareSerial (Serial): RX0=D0 (conectar desde C.TX=D5), TX0=D1 (PC)

// Buffers
char lineB[16]; byte idxB = 0;
char lineC[16]; byte idxC = 0;

inline void setLed(byte pin, char ch) {
  if (ch == '0' || ch == '1') digitalWrite(pin, ch == '1' ? HIGH : LOW);
}

inline void applyLine(byte pin, const char* s) {
  // Acepta "0"/"1" o "X,0|1"
  if ((s[0]=='0'||s[0]=='1') && s[1]=='\0') { setLed(pin, s[0]); return; }
  if (s[0] && s[1]==',' && (s[2]=='0'||s[2]=='1') && s[3]=='\0') { setLed(pin, s[2]); }
}

inline void drainAlt(AltSoftSerial& port, char* buf, byte& idx, byte ledPin) {
  while (port.available()) {
    char c = (char)port.read();
    if (c=='\n' || c=='\r') { if (idx) { buf[idx]='\0'; applyLine(ledPin, buf); idx=0; } }
    else if (idx < sizeof(lineB)-1) buf[idx++] = c; else idx = 0;
  }
}

inline void drainHW(char* buf, byte& idx, byte ledPin) {
  while (Serial.available()) {
    char c = (char)Serial.read();
    if (c=='\n' || c=='\r') { if (idx) { buf[idx]='\0'; applyLine(ledPin, buf); idx=0; } }
    else if (idx < sizeof(lineC)-1) buf[idx++] = c; else idx = 0;
  }
}

void setup() {
  pinMode(LED_B, OUTPUT); digitalWrite(LED_B, LOW);
  pinMode(LED_C, OUTPUT); digitalWrite(LED_C, LOW);

  UART_B.begin(BAUD);     // B -> A via AltSoftSerial (RX=8)
  Serial.begin(BAUD);     // C -> A via HardwareSerial (RX0=D0)

  // No usar prints por Serial para no mezclar con datos de C
}

void loop() {
  drainAlt(UART_B, lineB, idxB, LED_B);
  drainHW(lineC, idxC, LED_C);
}
~~~
{% endraw %}

# Dispositivo B

{% raw %}
~~~c++
#include <Arduino.h>
#include <SoftwareSerial.h>

constexpr unsigned long BAUD = 57600;
constexpr byte BTN_PIN = 2;           // a GND (INPUT_PULLUP)
constexpr unsigned long DEBOUNCE_MS = 30;
constexpr unsigned long HEARTBEAT_MS = 1000;

// TX -> A.RX_B (D8): usamos D9 como TX; RX dummy D7
SoftwareSerial UART_OUT(7, 9);        // (RX, TX) RX no usado

inline void sendVal(bool v){ UART_OUT.write(v?'1':'0'); UART_OUT.write('\n'); }

void setup(){
  pinMode(BTN_PIN, INPUT_PULLUP);
  UART_OUT.begin(BAUD);
}
void loop(){
  static int last=-1; static unsigned long tLast=0;
  static bool state=false; static unsigned long lastBeat=0;
  unsigned long now=millis();
  int b=(digitalRead(BTN_PIN)==LOW)?1:0;

  if(b!=last && (now-tLast)>DEBOUNCE_MS){ tLast=now; last=b; state=(b==1); sendVal(state); }
  if(now-lastBeat>=HEARTBEAT_MS){ lastBeat=now; sendVal(state); }
}
~~~
{% endraw %}

# Dispositivo c

{% raw %}
~~~c++
#include <Arduino.h>
#include <SoftwareSerial.h>

constexpr unsigned long BAUD = 57600;
constexpr byte BTN_PIN = 3;           // a GND (INPUT_PULLUP)
constexpr unsigned long DEBOUNCE_MS = 30;
constexpr unsigned long HEARTBEAT_MS = 1000;

// TX -> A.RX0 (D0): usamos D5 como TX; RX dummy D11
SoftwareSerial UART_OUT(11, 5);       // (RX, TX) RX no usado

inline void sendVal(bool v){ UART_OUT.write(v?'1':'0'); UART_OUT.write('\n'); }

void setup(){
  pinMode(BTN_PIN, INPUT_PULLUP);
  UART_OUT.begin(BAUD);
}
void loop(){
  static int last=-1; static unsigned long tLast=0;
  static bool state=false; static unsigned long lastBeat=0;
  unsigned long now=millis();
  int b=(digitalRead(BTN_PIN)==LOW)?1:0;

  if(b!=last && (now-tLast)>DEBOUNCE_MS){ tLast=now; last=b; state=(b==1); sendVal(state); }
  if(now-lastBeat>=HEARTBEAT_MS){ lastBeat=now; sendVal(state); }
}
~~~
{% endraw %}