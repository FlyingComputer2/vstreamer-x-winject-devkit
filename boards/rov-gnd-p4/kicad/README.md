# ROV-GND-P4 — Initial KiCad schematic (A0 draft)

Open `rov-gnd-p4.kicad_sch` in KiCad 9 or later. It uses an embedded `DEVKIT` symbol library and a project-local copy of the official Espressif ESP32-P4 package footprint. **This is an electrically named and symbolized first pass, not an ERC-clean or manufacturing-ready circuit.** The mechanical constraint is **50 × 50 mm** but no PCB routing has been completed.

## Grounding and revision

- Primary comparison schematic: [Waveshare ESP32-P4-WIFI6-DEV-KIT](https://files.waveshare.com/wiki/ESP32-P4-WIFI6-DEV-KIT/ESP32-P4-WIFI6-DEV-KIT.pdf), schematic **page 1**; Waveshare board-layout drawing pages 2–3 are **not copied**.
- Use Espressif's [current ESP32-P4 schematic checklist](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32p4/schematic-checklist-esp32p4.html) for **v3.x** new designs, especially core supply, VDDO_PSRAM, USB PHY, SDMMC and reset timing.
- U1 pin names/numbers are based on [official Espressif ESP32-P4X symbol](https://github.com/espressif/kicad-libraries/blob/main/symbols/Espressif.kicad_sym); project library was synthesized with the same official **105 package-pad definitions**. QFN footprint is from [Espressif kicad-libraries](https://github.com/espressif/kicad-libraries/blob/main/footprints/Espressif.pretty/ESP32-P4.kicad_mod), from which the copied local footprint is derived.
- **Physical pad 54 = VDD_HP_1 (1.2 V!)** on v3.x; do **not** confuse it with **GPIO54 = physical pad 98**. The existing GPIO expansion header intentionally carries GPIO54.

## What is in the schematic

- U1 complete P4X package with all named power pins, GPIOs and dedicated MIPI/USB/flash pins; embedded official pin numbers.
- QSPI flash U7 and flash CS pull-up, 40 MHz Y1 and placeholder crystal load capacitors.
- 5 V input J12; 3.3 V buck U2 **TBD**; core buck U3 **TLV62569DBVR pending verified transistor/feedback network**; protected servo, USB and radio power switch U4 **TBD**.
- Camera J1 Raspberry Pi-style 15-contact FPC pinout, radio J2 15-contact custom SDIO (the two are NOT interchangeable).
- SDMMC0 microSD J3 on underside/edge, USB2 HS Type-C J4, J5/J6/J7 UART, J8 I2C, J9 SPI, J10 post-magnetics Ethernet, J11 full 3×13 GPIO/servo header, J13 radio power.
- Placeholder RMII PHY U5, transformer/magnetics T1, and UART0 auto-download controller U8 with **clear TBD values**.
- 3.3 V/1.2 V/PSRAM/MIPI/flash rail labels, selected bulk/decoupling capacitors and SD/radio pull-up resistors.

## Unresolved HIGH-PRIORITY items

1. **GPIO35 bootstrap vs RMII TXD1 conflict.** GPIO35 high = normal boot; low + GPIO36 high enters Joint Download. The schematic now deliberately **keeps `BOOT_GPIO35` separate** from the PHY-side placeholder **`RMII_TXD1_UNASSIGNED`** to avoid a known unsafe connection. Assign a strap-safe, available output GPIO through a validated EMAC pin mux or isolate the PHY after reset; do not fabricate with the PHY output unresolved. See Espressif boot-mode table.
2. **New silicon core DCDC:** TLV62569DBVR choice follows Espressif's verified models, but **499 kΩ feedback resistors and 22 pF feedforward capacitor for v3.x MUST be implemented**, the inductor/input-output capacitors chosen per datasheet and the DCDC EN/FB topology checked. U3 is a pin-equivalent *planning block*, not a TLV62569 schematic.
3. **USB-C power and CC:** J4 D± nets are connected to P4's dedicated HS PHY, but CC1/CC2 go to a **yet unselected Type-C DRP controller**, and VBUS to a **yet unselected protected power switch**. Do NOT connect the USB shell's VBUS to +5 V directly or assemble without this circuitry. Low-cap ESD required.
4. **Camera rail/sensor:** J1 is a *proposed* 15-pin Raspberry Pi camera contract with camera control/I²C; confirm selected RPi camera module FPC orientation and whether reset/GPIO pin 11 is truly supported. CSI bias resistor (REXT), required power supply and camera sensor voltage not yet detailed.
5. **SDMMC/SDIO:** slot0 microSD GPIO39–44 is datasheet-backed; P4 GPIO14–19 to C5 **SDIO slave is custom and not validated**. Add source damping and CMD/DAT pull-ups and verify IO rail domain and 100 mm FPC signal quality.
6. **Flash and external PSRAM:** ESP32-P4NRW32X uses in-package 32 MB PSRAM but needs external QSPI flash. Verify exact flash ordering code and supply.
7. **MIPI supply:** U1 VDD_MIPI_DPHY connected to `MIPI_2V5` output of VDDO_3, **only after regulator firmware configuration**. Validate under power-up and reset; add CSI/DSI 4.02 kΩ REXT resistors after verifying circuit.
8. **Reset and UART control:** GPIO35/36/37/38 are strap pins. U8 is NOT a transistor implementation. Add approved Espressif 2-transistor auto-program circuit, DTR/RTS polarities and 10 kΩ/1 µF CHIP_PU network as tested.
9. **Ethernet:** U5 and T1 are *not selected components*. Choose PHY and RJ-less isolated line interface, RMII REFCLK source, routing and ESD.
10. **Space / footprint:** 50×50 mm, four corner M2 holes, north CSI, south SDIO, standing GH, east 3×13 GPIO and edge microSD constrain routing; no PCB file is asserted as valid layout.
11. **ERC:** unresolved blocks/net labels and power-driver flags mean ERC errors are expected. **Do not use for fabrication.**

## Explicit boundaries

- No onboard USB FS/JTAG receptacle, USB-to-UART bridge or SPI-EXT GH connector.
- GPIO24/25 are on the GPIO expansion header for optional native FS/JTAG through an **external USB-C breakout**.
- This is **not a reproduction of Waveshare's onboard C6**: that radio is replaced with the separate 15-pin FFC SDIO-to-C5 card.
- GPIO mapping beyond the verified USB FS, UART0, SDMMC0, silicon-dedicated PHYs and boot pins remains **provisional**.

## Project files

`rov-gnd-p4.kicad_sch` — actual KiCad schematic with embedded symbols and net labels

`DEVKIT.kicad_sym`, `sym-lib-table` — project-local library

`ESPRESSIF_LOCAL.pretty/ESP32-P4.kicad_mod`, `fp-lib-table` — official P4 footprint copied locally (Espressif)

`rov-gnd-p4.kicad_pro` — minimal KiCad project settings, regenerated by KiCad on open

Note: KiCad CLI/GUI is not available in this environment, so this draft was structurally checked (balanced S-expressions, 105 distinct P4 package pads, duplicated net-label continuity), **not loaded for ERC or a graphical render**.
