# Open hardware items before schematic freeze

1. **Pinout / strapping audit:** verify every ESP32-P4 pad, GPIO35 boot/EMAC interactions, CHIP_PU, boot straps, IO power domains, fixed microSD/USB/CSI mapping, and 0/1 handling. Draft map is not approved.
2. **UART0 control interface:** validate DTR/RTS auto-download transistor network and external programming adapter signal conventions. Confirm 6-pin GH pin assignment.
3. **I²C relocation:** camera 20/21, external 26/27 are *proposed*; check if GPIO22/23 and GPIO54 truly provide the claimed ADC functions on chosen P4 revision.
4. **Ethernet:** exact PHY, reference clock design, 100BASE-TX magnetics/ESD, four-contact JST-GH cable pinout and PHY strap/reset resistor design.
5. **USB:** only one onboard USB-C for HS OTG; confirm CC role controller, host-mode VBUS current-limit/discharge. Native FS/JTAG GPIO24/25 go to 3×13 GPIO header only (no onboard FS USB-C). For an external adapter confirm GND, CC pull-downs, ESD, short D± wires and prevention of cable-VBUS backfeeding the board 5V servo rail.
6. **SPI:** one 6-pin JST-GH for 3V3/GND/CLK/MOSI/MISO/CS0; optional CS1–CS5 are on the 3×13 GPIO header only (no SPI-EXT connector). Confirm ESP-IDF support for six hardware chip-selects or use software CS fallback.
7. **RF card:** QPF4507 pin-level reference, C5 TX/RX switching, 2.4 GHz unused port termination, fast detector selection and power monitors.
8. **Power tree:** input range and connectors; host and radio current budgets, sequencing, brownout, back-power prevention, thermals.
9. **Mechanical:** **hard 50 × 50 mm square**; eight **top-entry JST-GH** connectors including UART0/Ethernet/power, only USB-C side-entry; underside north-edge microSD, 4 corner M2 holes at (3.5,3.5)/(46.5,3.5)/(3.5,46.5)/(46.5,46.5). Fit-test the manufacturer footprints and mated plugs, CSI/SDIO latches, underside card insertion, GPIO header and screw-head clearances.
10. **Manufacturing/routability:** assess **6-layer** minimum candidate for dense 50 mm design; exact ESP32-P4 flash/PSRAM, 100BASE-TX PHY/magnetics, full power rails, ESD, GH pads, 3×13 header and microSD must all fit. MIPI CSI crossing upper GH bank and overlapping opposite-face SMD footprints require layout verification. Then DFM, BOM, PCB house, CAD libraries and license.

11. **KiCad initial schematic:** [open project](../boards/rov-gnd-p4/kicad/README.md) and run KiCad ERC, review power pins and pin mux. **Not yet ERC-tested**; placeholders for 3.3V buck, TLV62569 core buck network, USB role/protection, PHY/magnetics and auto-boot require real components.
12. **v3.x distinction:** physical pad 54 is `VDD_HP_1` (1.2V); do not confuse with usable `GPIO54` on physical pad 98. Check MCU package identity/revision for all BOM substitutes.
