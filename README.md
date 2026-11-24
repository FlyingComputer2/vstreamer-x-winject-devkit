# VStreamer × Winject Development Kit

A modular development platform for **VStreamer** (ESP32-P4 camera / H.264 host) and **Winject** (replaceable ESP32-C5 5 GHz radio).

**Project stage:** Rev A architecture and connector contract. Schematic, BOM, fabrication files, silicon pin validation, and RF compliance tests are **not yet complete**.

## Boards

- [`boards/rov-gnd-p4/`](boards/rov-gnd-p4/) — main ESP32-P4 camera / encoder carrier.
- [`boards/winject-radio-c5/`](boards/winject-radio-c5/) — replaceable ESP32-C5 radio, QPF4507 FEM, fast RF detector.

## Modular connection

- **Data:** native single-ended **4-bit SDIO** (DAT0–DAT3, CMD, CLK) over standard **15-contact, 1.0 mm pitch Raspberry Pi camera-type FFC/FPC**. Six signals and nine grounds; 100 mm cable target. No differential transceivers.
- **Power:** separate JST power connector; **no power through the FPC**. Exact power connector part/current and voltage protection to be confirmed.
- The C5 card has **its own flashing/debug UART, BOOT/RESET, and power regulation**.
- Cards are **not hot-swappable**; always power off before replacement.

See [SDIO cable contract](docs/radio-sdio-interface.md).

## P4 carrier features

- ESP32-P4, camera CSI-2 (15-pin Raspberry Pi-style connector), microSD slot.
- **One onboard USB-C** for VStreamer USB HS OTG (host/device). Native USB FS/JTAG has **no onboard USB-C**: GPIO24 (D−) and GPIO25 (D+) are exposed on the 3×13 header for an optional external USB-C adapter.
- UART0 flashing/logging via **6-pin JST-GH** with external auto-reset/boot controls; UART1 MSP DisplayPort; UART2 CRSF / S.Port via individual 4-pin JST-GH connectors.
- One external I²C bus **separate** from the camera's I²C.
- One external GP-SPI2 bus via a **single 6-pin JST-GH** connector labelled **SPI** (3V3, GND, CLK, MOSI, MISO, CS0). Optional CS1–CS5 are available **only on the 3×13 GPIO/servo header**, with no separate SPI-EXT connector and no shared IRQ.
- 100BASE-TX Ethernet with onboard PHY and magnetics; four-wire custom JST-GH twisted-pair cable-side connection.
- 3×13 GND/5V/GPIO servo-style header exposing CS1–CS5, GPIO33 spare, GPIO22/23/54 and optional FS/JTAG GPIO24/25.

## Design documents

- [80 × 80 mm mixed-orientation JST-GH board placement study](docs/placement-rev-a.md) ([dimensioned 1:1 SVG](docs/rov-gnd-p4-placement-rev-a.svg)); housing dimensions and sample courtyards to physical scale; not yet verified for mated cable access and final component footprints.

- [Architecture and connector decisions](docs/architecture.md)
- [Radio SDIO interface and FPC pinout](docs/radio-sdio-interface.md)
- [Draft GPIO allocation — **not signed off**](docs/gpio-map-draft.md)
- [Incremental bring-up checklist](docs/bringup.md)
- [Open hardware decisions / validations](docs/open-items.md)

> **Warning:** The GPIO map is a *conversation-level proposal*, not an electrically verified P4 pinout. Check the exact ESP32-P4 silicon revision, package, strapping pins, USB pins, SDMMC mapping, PHY reference clock, peripheral GPIO-matrix restrictions, and Espressif's hardware design guidelines before schematic or PCB release.

No license selected yet.
