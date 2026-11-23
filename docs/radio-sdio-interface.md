# Radio interface: single-ended SDIO over 15-pin camera-type flex

**Decision:** standard Raspberry Pi 15-pin / 1.0 mm pitch camera FFC cable and connectors mechanically, but **custom electrical pinout** carrying SDIO, not CSI. Never attach a camera to the radio connector.

## Proposed pinout

| Contact | Net | Contact | Net | Contact | Net |
|---:|---|---:|---|---:|---|
| 1 | GND | 2 | DAT0 | 3 | GND |
| 4 | DAT1 | 5 | GND | 6 | DAT2 |
| 7 | GND | 8 | DAT3 | 9 | GND |
| 10 | CMD | 11 | GND | 12 | CLK |
| 13 | GND | 14 | GND | 15 | GND |

Six signals, nine grounds. Alternating grounds isolate adjacent *conductor positions*; this is **not a continuous shielding plane** in a conventional FFC. Short PCB routes and continuous PCB ground planes remain necessary.

## Electrical contract (target, not validated)

- ESP32-P4 is SDIO host; ESP32-C5 is SDIO slave.
- 3.3 V logic signalling; voltage compatibility and pull-ups to be confirmed against actual chip/power domain.
- 50 MHz, 4-bit throughput target; validate at 10, 25 and 50 MHz with long-duration error counters.
- Cable target 100 mm, standard 15-pin RPi-type FFC (exact cable orientation and contact configuration TBD).
- Power is on a **separate JST connector**. Do not put power or control pins on SDIO flex.
- The C5 card provides its own UART flashing/debug and BOOT/RESET.
- Before radio power-off, ensure P4 SDIO is inactive / high-Z and cannot back-power the radio.
- Optional damping / 0-Ω tuning footprints for CLK and other lines, sized after probing; do not indiscriminately insert source resistors on bidirectional lines.
- Verify contact numbering, mating face (top/bottom), cable conductor crossover and connector orientation in both PCB footprints before fabrication.

## Required validation

1. Check exact 15-pin connector and cable manufacturer part numbers, mating-cycle specification, and latch operation.
2. Capture SDIO CLK/CMD/DAT waveforms with appropriate low-capacitance probes.
3. Validate bit errors, CRC behavior, and thermal/hot-load reliability at 100 mm / 50 MHz.
4. Confirm ESP32-C5 silicon revision with supported SDIO slave mode and host firmware integration.
5. Confirm sharing the same 15-pin cable type as CSI is safe under lab handling; use distinct labels and physical placement.
