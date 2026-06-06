# Mirage AC Protocol (EICON121-0 / KKG27A-C11)

This repository documents the infrared protocol used by the EICON121-0 mini-split air conditioner and KKG27A-C11 remote control.

The protocol was reverse engineered using an ESP32 running ESPHome with an IR receiver and transmitter. All values documented below have been verified through testing on physical hardware.

## Features Decoded

* Power On / Off
* Temperature Setpoint
* Cool / Heat / Dry / Auto / Fan Modes
* Fan Speed (Auto, Low, Medium, High)
* Turbo
* Quiet
* Sleep
* Health
* Clean
* Lock
* H-Sweep
* V-Sweep
* Display Toggle
* Celsius / Fahrenheit
* ON Timer
* OFF Timer

## Hardware Tested

| Component      | Model      |
| -------------- | ---------- |
| Indoor Unit    | EICON121-0 |
| Remote Control | KKG27A-C11 |
| Controller     | ESP32      |
| Software       | ESPHome    |

## Protocol Structure

Packet Length: 14 Bytes

| Byte | Function                      |
| ---- | ----------------------------- |
| 0    | Header (0x56)                 |
| 1    | Temperature / Setpoint        |
| 2    | Unknown                       |
| 3    | Quiet                         |
| 4    | Mode + Fan                    |
| 5    | Swing / Display / Power State |
| 6    | Health / Sleep / Heat / Lock  |
| 7    | Clean                         |
| 8    | ON Timer Hours                |
| 9    | Temperature Units             |
| 10   | OFF Timer Hours               |
| 11   | Unknown                       |
| 12   | Unknown                       |
| 13   | Unknown                       |

## Temperature Encoding

Temperature appears to be stored as:

Byte1 = Temperature + 0x5C

Examples:

| Temperature | Value |
| ----------- | ----- |
| 20°C        | 0x70  |
| 21°C        | 0x71  |

## Byte Definitions

### Byte 3

| Value | Function |
| ----- | -------- |
| 0x01  | Quiet    |

### Byte 4 - Mode

| Value | Function |
| ----- | -------- |
| 0x10  | Heat     |
| 0x20  | Cool     |
| 0x30  | Dry      |
| 0x40  | Auto     |
| 0x50  | Fan      |

### Byte 4 - Fan Offset

| Value | Function |
| ----- | -------- |
| +0    | Auto     |
| +1    | High     |
| +2    | Low      |
| +3    | Medium   |

Examples:

| Value | Meaning       |
| ----- | ------------- |
| 0x20  | Cool + Auto   |
| 0x21  | Cool + High   |
| 0x22  | Cool + Low    |
| 0x23  | Cool + Medium |

### Byte 5

| Value | Function                    |
| ----- | --------------------------- |
| 0x01  | H-Sweep                     |
| 0x02  | V-Sweep                     |
| 0x03  | H + V Sweep                 |
| 0x04  | Display Toggle              |
| 0xC0  | Power Off / Power State Off |

### Byte 6

| Value | Function  |
| ----- | --------- |
| 0x02  | Health    |
| 0x08  | Sleep     |
| 0x40  | Heat Flag |
| 0x80  | Lock      |

### Byte 7

| Value | Function |
| ----- | -------- |
| 0x40  | Clean    |

### Byte 8

ON Timer Hours

Examples:

| Value | Meaning            |
| ----- | ------------------ |
| 0x03  | Turn ON in 3 Hours |
| 0x05  | Turn ON in 5 Hours |

### Byte 9

| Value | Function   |
| ----- | ---------- |
| 0x00  | Celsius    |
| 0xC0  | Fahrenheit |

### Byte 10

OFF Timer Hours

Examples:

| Value | Meaning             |
| ----- | ------------------- |
| 0x01  | Turn OFF in 1 Hour  |
| 0x05  | Turn OFF in 5 Hours |

## Example Packets

### Cool, 20°C, Auto Fan

56:70:00:00:20:00:00:00:00:00:00:00:00:00

### Power Off

56:70:00:00:20:C0:00:00:00:00:00:00:00:00

### Turbo

56:70:00:00:20:00:00:00:80:00:00:00:00:00

### Clean

56:70:00:00:20:00:00:40:00:00:00:00:00:00

## Remaining Unknown Fields

The following bytes are accepted by the indoor unit but have not yet been fully decoded:

* Byte 2
* Byte 11
* Byte 12

## Repository Contents

* Mirage_AC_IR_Protocol_Documentation.pdf
* Mirage_AC_Captures.txt
* README.md

## License

MIT License
