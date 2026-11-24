# ROV-GND-P4

Standalone ESP32-P4 camera/encoder host board. Work-in-progress architecture; CAD and BOM not yet started.

- Camera CSI-2 (15-pin); independent I²C.
- MicroSD on native SDMMC.
- 15-pin SDIO FPC to replaceable Winject C5 radio; external radio power switching.
- One onboard USB-C for HS OTG. Native USB FS/JTAG uses GPIO24 D− and GPIO25 D+ on the 3×13 header with an optional external USB-C adapter; no onboard FS receptacle.
- UART0 6-pin JST-GH flash/log with external DTR/RTS auto-control; UART1 MSP; UART2 CRSF/S.Port.
- One standard 6-position JST-GH connector labelled SPI (3V3, GND, CLK, MOSI, MISO, CS0); optional CS1–CS5 and spare GPIO33 on the 3×13 GPIO/servo header. No SPI-EXT connector.
- One external I²C (separate from camera).
- 100BASE-TX Ethernet PHY + onboard magnetics + four-contact JST-GH cable interface.
- 3×13 GND/5V/Signal servo-style GPIO expansion; see [draft map](../../docs/gpio-map-draft.md).
