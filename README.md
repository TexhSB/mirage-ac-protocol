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

## Temperature Encoding

Byte 1 stores the temperature.

Formula:

```text
Byte1 = Temperature + 0x5C
```

Examples:

| Temperature | Value |
|------------|--------|
| 20°C | 0x70 |
| 21°C | 0x71 |

## Example Packet

Cool, 20°C, Auto Fan:

56:70:00:00:20:00:00:00:00:00:00:00:00:00

## Byte Definitions

### Byte 3

| Value | Function |
|---------|---------|
| 0x01 | Quiet |

### Byte 5

| Value | Function |
|---------|---------|
| 0x01 | H-Sweep |
| 0x02 | V-Sweep |
| 0x04 | Display Toggle |
| 0xC0 | Power Off |

### Byte 6

| Value | Function |
|---------|---------|
| 0x02 | Health |
| 0x08 | Sleep |
| 0x40 | Heat Flag |

### Byte 7

| Value | Function |
|---------|---------|
| 0x40 | Clean |

### Byte 8

| Value | Function |
|---------|---------|
| 0x80 | Turbo |


## Hardware Tested

Indoor Unit: EICON121-0
Remote Control: KKG27A-C11

Protocol decoded using:
- ESP32
- ESPHome
- IR receiver on GPIO21
- ESPHome Mirage protocol decoder

## Notes

- Display appears to be a toggle command rather than a tracked state.
- Quiet mode automatically forces Low Fan speed.
- Turbo mode is independent of Health mode.
- H-Sweep and V-Sweep are bit flags and may be combined (0x03).
- Power Off is encoded using Byte 5 = 0xC0.
