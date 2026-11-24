# Draft ESP32-P4 GPIO map — NOT VERIFIED

This captures the *latest design discussion*, partially crosschecked against the official v3.x Espressif ESP32-P4X symbol and schematic checklist; a complete signal-mux and boot-strap audit is still required. A full pin-mux/strap audit is required before PCB routing. In particular, BOOT, CHIP_PU/EN, Ethernet GPIO35, SDMMC and USB dedicated pins have been sources of confusion; **do not treat this document as a released netlist**.

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
| Optional SPI CS1..CS5; GPIO33 spare (3×13 GPIO header) | 4 / 5 / 6 / 13 / 32 / 33 | Provisional, no separate SPI-EXT connector |
| Native USB FS/JTAG D−/D+ | 24 / 25 | 3×13 GPIO header only; optional external USB-C adapter; no dedicated onboard FS port |
| USB HS OTG D−/D+ | Dedicated USB pins | Verify actual package nets |
| Ethernet RMII CRS_DV, RXD0, RXD1 | 28 / 29 / 30 | Provisional |
| Ethernet RMII REF_CLK, TX_EN | 50 / 49 | Provisional |
| Ethernet RMII TXD0, TXD1 | 34 / 35 | **Draft net tied:** TXD1 on GPIO35, shared with BOOT input and its 10 kΩ pull-up; PHY input/loading and boot/download timing still require verification |
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
| 4 | IN/OUT | Optional SPI CS1 (GPIO header) | Shared with SPI; alternate mux/touch functions require silicon verification |
| 5 | IN/OUT | Optional SPI CS2 (GPIO header) | Shared with SPI |
| 6 | IN/OUT | Optional SPI CS3 (GPIO header) | Shared with SPI |
| 13 | IN/OUT | Optional SPI CS4 (GPIO header) | Shared with SPI |
| 22 | IN/OUT | Free / analog candidate | **ADC capability not verified** |
| 23 | IN/OUT | Free / analog candidate | **ADC capability not verified** |
| 24 | IN/OUT | USB FS D− / GPIO | No onboard FS port; external USB adapter must be disconnected for unrelated GPIO loads |
| 25 | IN/OUT | USB FS D+ / GPIO | Must isolate FS USB connector in GPIO mode |
| 32 | IN/OUT | Optional SPI CS5 (GPIO header) | Shared with SPI |
| 33 | IN/OUT | GPIO header spare | Shared connector |
| 54 | IN/OUT | Free / analog candidate | **ADC capability not verified** |

**Note:** This table is a *plan*, not proof of pin availability. Any 3.3 V GPIO exposure must respect actual IO power domains, input limits, and bootstrap constraints. Shared GPIO cannot be used independently by the SPI and the external device at the same time.

## Auto-flash UART0 wiring

The 6-pin JST-GH pinout was proposed as: 1=3V3 reference, 2=GND, 3=TX0, 4=RX0, 5=DTR control, 6=RTS control. DTR/RTS are signals from the external adapter, not MCU GPIO functions. Include an Espressif-style transistor control network for BOOT and CHIP_PU (EN), and retain buttons. Exact strapping pin, active polarity, and BOOT/EN electrical implementation **must** be verified before drawing the circuit.

## Proposed 3 × 13 GPIO expansion header (39 contacts)

**Decision:** 3 columns × 13 rows. This supersedes a generic unspecified GPIO expansion header. Contact orientation, mechanical pitch and actual footprint/pin-1 numbering still require selection. Table is arranged in logical **rows**, not manufacturer's pin numbers.

| Row | Column 1 | Column 2 | Column 3 |
|---:|---|---|---|
| 1 | GND | 3V3 | 3V3 |
| 2 | GND | 5V | 5V |
| 3 | GND | 5V | GPIO4 / Optional SPI CS1 (GPIO header) |
| 4 | GND | 5V | GPIO5 / Optional SPI CS2 (GPIO header) |
| 5 | GND | 5V | GPIO6 / Optional SPI CS3 (GPIO header) |
| 6 | GND | 5V | GPIO13 / Optional SPI CS4 (GPIO header) |
| 7 | GND | 5V | GPIO32 / Optional SPI CS5 (GPIO header) |
| 8 | GND | 5V | GPIO33 / GPIO header spare |
| 9 | GND | 5V | GPIO22 (general / ADC candidate) |
| 10 | GND | 5V | GPIO23 (general / ADC candidate) |
| 11 | GND | 5V | GPIO54 (general / ADC candidate) |
| 12 | GND | 5V | GPIO25 / USB FS D+ |
| 13 | GND | 5V | GPIO24 / USB FS D− |

Totals: **13 GND + 13 5V + 2 3V3 + 11 signal = 39 contacts**. All 11 signal pins are planned as digital input/output capable, pending package/IO-domain validation; some have alternate functions, so not all are simultaneously free.

### Electrical and mechanical precautions

- Rows 3–13 use the conventional **GND / 5 V / signal** servo-header arrangement. Label the polarity and pin 1 clearly; GPIO signals remain 3.3 V-only (not 5 V tolerant). Exact header mechanical pitch, footprint and orientation are TBD.
- 5 V is a **shared rail** repeated along the middle column and row 2, *not* thirteen independent regulated outputs. Design trace width, connector current per contact, total current limit and any protective fuse/eFuse accordingly. Same for 3V3.
- GPIO24 D− and GPIO25 D+ are exposed **only on this header** for optional native USB FS/JTAG via an external USB-C adapter. They are also general GPIOs when USB is inactive, but **cannot be used for unrelated GPIO functions simultaneously with USB**. There is no onboard FS USB-C port or onboard USB/GPIO switching path. External adapter: short D± wiring, GND, Type-C CC resistors and ESD as needed; **do not tie external USB VBUS to the shared 5 V servo rail**.
- **CS1–CS5 are connected only to this 3×13 GPIO header**, not to a separate SPI-EXT connector. Each pin can be configured as a SPI chip select or another GPIO function, not both at once. To attach a second SPI device, share CLK/MOSI/MISO/GND from the main SPI JST-GH and bring its CS wire from this header.
- This header is a design allocation only, not a finalized electrical netlist or manufacturing drawing; check actual pinout, boot straps and package capabilities before routing.

## Silicon revision and initial KiCad schematic

The [initial KiCad schematic](../boards/rov-gnd-p4/kicad/rov-gnd-p4.kicad_sch) uses **ESP32-P4NRW32X v3.x** and official Espressif physical-pad mapping. On v3.x, **physical pin 54 is VDD_HP_1 (1.2 V supply)**, while **GPIO54 remains available on physical pin 98** and is the proposed ADC2_CH5 pin of the expansion header. These are distinct.

GPIO35, GPIO36, GPIO37 and GPIO38 participate in boot strapping. **GPIO35=high** is normal SPI boot; GPIO35=low and GPIO36=high selects joint download. The KiCad draft now ties GPIO35/BOOT to Ethernet RMII TXD1 on the same net (`RMII_TXD1_BOOT_GPIO35`), consistent with the Waveshare ESP32-P4-WIFI6-DEV-KIT IP101-family Ethernet mapping. Because GPIO35 is also the boot-mode strap, retain its pull-up, ensure the PHY TXD1 input does not pull the net low during strap sampling, and verify the auto-programming circuit and reset timing. The PHY and magnetics remain conceptual placeholders: this is a **logical net connection, not approval for hardware fabrication**. Camera SDMMC0, UART0 and USB data pins have higher confidence than custom flexible GPIO-matrix mappings. [Espressif schematic checklist](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32p4/schematic-checklist-esp32p4.html).


### 2026-11-24 verified reference check

- [Waveshare ESP32-P4-WIFI6-DEV-KIT Rev 1.2 schematic](https://files.waveshare.net/wiki/ESP32-P4-WIFI6-DEV-KIT/ESP32-P4-WIFI6-DEV-KIT.pdf): example P4 + IP101-family Ethernet PHY, but its onboard ESP32-C6 wiring must **not** replace our independent ESP32-C5 SDIO connector.
- [Espressif ESP32-P4 hardware schematic checklist](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32p4/schematic-checklist-esp32p4.html): RMII TXD1 is limited to GPIO35 or GPIO42; GPIO42 is dedicated microSD DAT3 on SDMMC slot 0 in our design, and GPIO35 needs high on normal boot.
- Only the shared RMII_TXD1/BOOT35 signal has been connected in this review. USB-C CC/OTG, genuine 1.2 V and 3.3 V supplies, Ethernet PHY/magnetics, UART auto-programming, and card power still need fully specified devices and electrical verification. Do not treat a clean label count as ERC sign-off.
