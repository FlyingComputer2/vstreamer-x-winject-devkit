# Incremental bring-up plan

## Stage 0 — schematic validation
- [ ] Choose precise ESP32-P4 and ESP32-C5 silicon ordering codes/revisions.
- [ ] Verify P4 power/PSRAM/flash reference circuit and all GPIO/strapping assignments.
- [ ] Lock FPC contact orientation and 6-pin JST-GH cable pin numbering.
- [ ] Verify USB-C FS and HS CC/VBUS protection and role-control scheme.

## Stage 1 — standalone ROV-GND-P4
- [ ] 3.3 V and core supply measurements; boot/reset behavior.
- [ ] UART0 auto-flash via external adapter, DTR/RTS controls, logs.
- [ ] USB FS/JTAG enumerate and flash; GPIO24/25 jumper mode.
- [ ] USB HS OTG device and host modes with power protection.
- [ ] Camera MIPI CSI and its independent I²C.
- [ ] microSD sustained read/write; check errors.
- [ ] External I²C, UART1 MSP, UART2 CRSF / S.Port, and SPI CS0 through the SPI JST-GH plus optional CS1–CS5 through the GPIO header.
- [ ] 100BASE-TX Ethernet PHY and isolated 4-wire cable connection.

## Stage 2 — standalone WINJECT-RADIO-C5
- [ ] Local C5 UART flash/log/BOOT/RESET.
- [ ] Radio power/current monitor and turn-on sequencing.
- [ ] Native radio RF baseline, bypass FEM when appropriate.
- [ ] QPF4507 control and RF power measurements into rated load.
- [ ] Fast detector response vs TX enable and actual RF onset.

## Stage 3 — integrated link
- [ ] Connect 100 mm FFC + separate radio JST power only while off.
- [ ] Validate SDIO 1-bit and 4-bit at 10/25/50 MHz.
- [ ] Stress DAT lines, CRC/error counters, boot/reset, brownout, hot-load power (without hot-plugging).
- [ ] Run VStreamer encoder and Winject radio end-to-end; track latency and throughput.
- [ ] A/B swap radio revisions with fixed host, antenna, supply, workload and test conditions.
