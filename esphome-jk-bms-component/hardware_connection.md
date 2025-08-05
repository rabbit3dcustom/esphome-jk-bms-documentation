# ESP32 Hardware connection and wiring


## Schematics

```
                              CONVERTER                          UART-TTL
┌──────────┐                ┌───────────┐                       ┌─────────┐
│          │<----- A  ----->│  SERIAL   │<-----------------Vcc--│         │<--Vcc
│  JK-BMS  │<----- B  ----->│  TTL TO   │                       │ ESP32/  │
│          │                │  RS485    │--RO---------------RX->│ ESP8266 │
│          │                │           │<-DI---------------TX--│         │
│          │                │ CONVERTER │<-DE-----+             │         │
│          │                │           │<-RE-----└---TALK PIN--│         │
│          │                │           │                       │         │
|          |<------GND----->|           |<----------GND-------->|         |<--GND
└──────────┘                └───────────┘                       └─────────┘

```
Description
- Vcc: Could be connected to 3.3v or 5v 


## RJ45

At the JKBMS Looking the connector from the bottom/back, the clip should be facing down.

```
Usable any of the two wirings:
┌─────────────────────────┐          ┌─────────────────────────┐
│                         │          │                         │
│ O  O  O  O  O  O  O  O  │   OR     │ O  O  O  O  O  O  O  O  │
│ 1  2  3  4  5  6  7  8  │          │ 1  2  3  4  5  6  7  8  │
└─────────────────────────┘          └─────────────────────────┘
  │  │  │                                             │  │  │   
  │  │  └──── GND                              GND ───┘  │  │   
  │  └─────── A                                  A ──────┘  │   
  └────────── B                                  B ─────────┘   
```

## Example
![image](./images/ESP32-Schematics.png)
