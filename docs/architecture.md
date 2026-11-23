# Rev A architecture and decisions

## ROV-GND-P4 host

| Interface | Rev A choice |
|---|---|
| MCU | ESP32-P4 (exact ordering code / revision TBD) |
| Camera | 15-contact, 1.0 mm pitch Raspberry Pi-style CSI-2 FPC |
| Camera I²C | Independent bus/controller from external I²C |
| microSD | Native SDMMC slot; 4-bit target |
| Radio data | SDIO 4-bit host, 15-contact FPC, 100 mm cable target |
| USB-C #1 | Native USB Full-Speed Serial/JTAG **peripheral**, not USB-to-UART bridge |
| USB-C #2 | USB 2.0 High-Speed OTG, VStreamer host/device |
| UART0 | Flash and logging; 6-pin JST-GH carrying 3V3 reference, GND, TX, RX, DTR, RTS (control-circuit mapping TBD) |
| UART1 | MSP DisplayPort; 4-pin JST-GH |
| UART2 | CRSF / S.Port; 4-pin JST-GH; native inversion/half-duplex plan, verify interface electrical voltage |
| Other UARTs | Not exposed |
| External I²C | Exactly one 4-pin JST-GH bus, independent of camera I²C |
| SPI | One GP-SPI2 bus, up to 6 CS signals intended; actual ESP-IDF driver behavior to validate |
| SPI connector | 1×6 standard JST-GH: 3V3, GND, CLK, MOSI, MISO, CS0 |
| Optional SPI CS | CS1–CS5 are exposed only on the 3×13 GPIO/servo header; **no SPI-EXT JST-GH connector** |
| SPI IRQ | None shared; device-specific interrupt may use a spare GPIO |
| Ethernet | 100BASE-TX PHY + *onboard* magnetics; four-contact JST-GH carries isolated twisted-pair cable signals |
| General GPIO | 3×13 GND/5V/Signal servo-style header; CS1–CS5, GPIO33 spare, GPIO22/23/54, and FS USB D± GPIO24/25 via selectable isolation |

## WINJECT-RADIO-C5 card

- ESP32-C5, a QPF4507 integrated 5 GHz FEM and a fast RF detector (provisional detector component selection).
- Independent UART programming/debug, local BOOT and RESET, and its own regulators.
- Separate JST power connection.
- Dedicated SDIO FPC (SDIO only, interleaved GND), no UART or power on the flex.
- Direct RF / FEM bypass population option recommended for initial bring-up.
- Power-off protection and current measurement required.

## Ethernet cable wiring

Connector is **not Ethernet RJ45** and is not intended to carry the PHY-side signals: onboard magnetics provide isolation before the 4-pin JST-GH cable port. Proposed cable-side mapping: pair A pins 1/2, pair B pins 3/4; exact polarity labelling, ESD topology and connector part need schematic review. Use two actual twisted pairs and 100 Ω differential layout on the PCB.

## USB separation

`USB FS / JTAG` is a built-in USB interface. `UART0 / FLASH` is a separate hardware UART on its own JST-GH; do not describe the FS USB port as a USB-UART bridge. `USB HS / OTG` is a second independent USB data interface.

## Scope

First validate each board independently, then connect SDIO, then run VStreamer streaming and RF tests. This is a replaceable prototype rather than an optimized integrated product.
