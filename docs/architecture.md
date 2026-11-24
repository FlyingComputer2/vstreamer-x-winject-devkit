# Rev A architecture and decisions

## ROV-GND-P4 host

| Interface | Rev A choice |
|---|---|
| MCU | ESP32-P4 (exact ordering code / revision TBD) |
| Camera | 15-contact, 1.0 mm pitch Raspberry Pi-style CSI-2 FPC |
| Camera I²C | Independent bus/controller from external I²C |
| microSD | Native SDMMC slot; 4-bit target |
| Radio data | SDIO 4-bit host, 15-contact FPC, 100 mm cable target |
| USB-C on board | **One port only:** USB 2.0 High-Speed OTG for VStreamer host/device |
| Native USB FS/JTAG | GPIO24 D− and GPIO25 D+ on 3×13 GPIO header for optional external USB-C adapter; no onboard FS receptacle |
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
| General GPIO | 3×13 GND/5V/Signal servo-style header; CS1–CS5, GPIO33 spare, GPIO22/23/54, and FS USB D± GPIO24/25, with no onboard FS USB connector to isolate |

## Mechanical envelope and connector-edge layout

Rev A placement: **50 × 50 mm hard square outline, preliminary component-envelope placement**. North: camera CSI-2. West: one USB-C HS OTG and six JST-GH ports in order UART0, UART1, UART2, I²C, SPI, Ethernet. East: 3×13 servo/GPIO. South: radio SDIO FPC, radio power JST, main 5 V input. See [placement draft](placement-rev-a.md) and [concept diagram](rov-gnd-p4-placement-rev-a.svg). All JST-GH ports, **including UART0, Ethernet, main 5 V input and radio power, are top-entry/standing**, clustered in two west-side banks around the single lying USB-C. North camera CSI FPC, south SDIO FPC, east 3×13 GPIO. The microSD is on the **bottom** with north-edge ejection for accessibility. Four corner M2 mounting holes are tentatively centered 3.5 mm from both adjacent edges. The nominal top connector rectangles do not overlap, but 3D cable/SMT courtyard clearance, missing components, power budget and high-speed routing have not been validated.

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

Native USB FS/JTAG remains built into the P4, but there is **no dedicated USB-C on the board for it**. GPIO24 D− and GPIO25 D+ are accessible on the 3×13 header for an external USB-C device adapter; the adapter needs GND, Type-C CC pull-downs, suitable ESD and appropriate VBUS handling. **Do not connect USB cable VBUS directly to the servo-header +5V rail.** UART0 / FLASH is a separate hardware UART on a 6-pin GH connector; no USB-to-UART bridge is installed. USB HS / OTG is the only onboard USB-C.

## Scope

First validate each board independently, then connect SDIO, then run VStreamer streaming and RF tests. This is a replaceable prototype rather than an optimized integrated product.
