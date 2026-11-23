# Draft ESP32-P4 GPIO map — NOT VERIFIED

This captures the *latest design discussion*, not a datasheet-checked pinout. A full pin-mux/strap audit is required before PCB routing. In particular, BOOT, CHIP_PU/EN, Ethernet GPIO35, SDMMC and USB dedicated pins have been sources of confusion; **do not treat this document as a released netlist**.

## Signal allocation proposals

| Function | GPIO(s) | Status |
|---|---|---|
| UART0 TX/RX (flash/log) | 37 / 38 | Provisional: check strap/pin capability |
| UART1 TX/RX (MSP DisplayPort) | 11 / 12 | Provisional |
| UART2 TX/RX (CRSF / S.Port) | 2 / 3 | Provisional |
| Camera I²C SDA/SCL | 20 / 21 | Provisional, separate controller |
| External I²C SDA/SCL | 26 / 27 | Provisional, moved from GPIO22/23 |
| Radio SDIO DAT0..DAT3 | 14 / 15 / 16 / 17 | Provisional |
| Radio SDIO CLK / CMD | 18 / 19 | Provisional |
| microSD DAT0..DAT3 | 39 / 40 / 41 / 42 | Provisional |
| microSD CLK / CMD | 43 / 44 | Provisional |
| SPI CLK / MOSI / MISO / CS0 | 9 / 8 / 10 / 7 | Provisional |
| SPI-EXT CS1..CS5 / SPARE | 4 / 5 / 6 / 13 / 32 / 33 | Provisional, optional GPIO |
| USB FS Serial/JTAG D−/D+ | 24 / 25 | Provisional, GPIO only when USB disconnected |
| USB HS OTG D−/D+ | Dedicated USB pins | Verify actual package nets |
| Ethernet RMII CRS_DV, RXD0, RXD1 | 28 / 29 / 30 | Provisional |
| Ethernet RMII REF_CLK, TX_EN | 50 / 49 | Provisional |
| Ethernet RMII TXD0, TXD1 | 34 / 35 | **Critical: boot strap conflict to review** |
| Ethernet MDC, MDIO, PHY_RESET | 31 / 52 / 51 | Provisional |
| Camera control GPIO | 45 | Provisional |
| microSD power enable | 47 | Provisional |
| USB HS role / VBUS enable | 46 / 48 | Provisional; controller may require different control wiring |
| Radio power enable | 53 | Provisional |
| Spare GPIO | 54 | Provisional; ADC claims not yet validated |
| GPIO0/1 | Reserved | Do not reassign without reviewing RTC/boot circuitry |
| GPIO36 | Reserved | Boot / strapping review |
| CHIP_PU / EN | Dedicated chip pin | Not GPIO; reset control |

## Proposed GPIO expansion (11 signals)

| GPIO | Digital direction | Assignment | Alternatives / cautions |
|---:|---|---|---|
| 4 | IN/OUT | SPI-EXT CS1 | Shared with SPI; alternate mux/touch functions require silicon verification |
| 5 | IN/OUT | SPI-EXT CS2 | Shared with SPI |
| 6 | IN/OUT | SPI-EXT CS3 | Shared with SPI |
| 13 | IN/OUT | SPI-EXT CS4 | Shared with SPI |
| 22 | IN/OUT | Free / analog candidate | **ADC capability not verified** |
| 23 | IN/OUT | Free / analog candidate | **ADC capability not verified** |
| 24 | IN/OUT | USB FS D− / GPIO | Must isolate FS USB connector in GPIO mode |
| 25 | IN/OUT | USB FS D+ / GPIO | Must isolate FS USB connector in GPIO mode |
| 32 | IN/OUT | SPI-EXT CS5 | Shared with SPI |
| 33 | IN/OUT | SPI-EXT SPARE | Shared connector |
| 54 | IN/OUT | Free / analog candidate | **ADC capability not verified** |

**Note:** This table is a *plan*, not proof of pin availability. Any 3.3 V GPIO exposure must respect actual IO power domains, input limits, and bootstrap constraints. Shared GPIO cannot be used independently by the SPI and the external device at the same time.

## Auto-flash UART0 wiring

The 6-pin JST-GH pinout was proposed as: 1=3V3 reference, 2=GND, 3=TX0, 4=RX0, 5=DTR control, 6=RTS control. DTR/RTS are signals from the external adapter, not MCU GPIO functions. Include an Espressif-style transistor control network for BOOT and CHIP_PU (EN), and retain buttons. Exact strapping pin, active polarity, and BOOT/EN electrical implementation **must** be verified before drawing the circuit.
