# Mirage AC Protocol (EICON121-0 / KKG27A-C11)

This repository documents the infrared protocol used by the EICON121-0 mini-split air conditioner and KKG27A-C11 remote control.

## Features Decoded

- Power On/Off
- Temperature
- Cool / Heat / Dry / Fan / Auto
- Fan Speed (Auto, Low, Medium, High)
- Turbo
- Quiet
- Sleep
- Health
- Clean
- H-Sweep
- V-Sweep
- Display Toggle

## Protocol Summary

Packet Length: 14 bytes

| Byte | Function |
|--------|--------|
| 0 | Header (0x56) |
| 1 | Temperature |
| 3 | Quiet |
| 4 | Mode + Fan |
| 5 | Sweep / Display / Power |
| 6 | Health / Sleep / Heat |
| 7 | Clean |
| 8 | Turbo |

## Example Packet

Cool, 20°C, Auto Fan:

56:70:00:00:20:00:00:00:00:00:00:00:00:00

Hardware Tested

Indoor Unit: EICON121-0
Remote Control: KKG27A-C11

Protocol decoded using:
- ESP32
- ESPHome
- IR receiver on GPIO21
- ESPHome Mirage protocol decoder
