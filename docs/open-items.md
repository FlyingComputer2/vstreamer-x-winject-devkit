# Open hardware items before schematic freeze

1. **Pinout / strapping audit:** verify every ESP32-P4 pad, GPIO35 boot/EMAC interactions, CHIP_PU, boot straps, IO power domains, fixed microSD/USB/CSI mapping, and 0/1 handling. Draft map is not approved.
2. **UART0 control interface:** validate DTR/RTS auto-download transistor network and external programming adapter signal conventions. Confirm 6-pin GH pin assignment.
3. **I²C relocation:** camera 20/21, external 26/27 are *proposed*; check if GPIO22/23 and GPIO54 truly provide the claimed ADC functions on chosen P4 revision.
4. **Ethernet:** exact PHY, reference clock design, 100BASE-TX magnetics/ESD, four-contact JST-GH cable pinout and PHY strap/reset resistor design.
5. **USB:** only one onboard USB-C for HS OTG; confirm CC role controller, host-mode VBUS current-limit/discharge. Native FS/JTAG GPIO24/25 go to 3×13 GPIO header only (no onboard FS USB-C). For an external adapter confirm GND, CC pull-downs, ESD, short D± wires and prevention of cable-VBUS backfeeding the board 5V servo rail.
6. **SPI:** one 6-pin JST-GH for 3V3/GND/CLK/MOSI/MISO/CS0; optional CS1–CS5 are on the 3×13 GPIO header only (no SPI-EXT connector). Confirm ESP-IDF support for six hardware chip-selects or use software CS fallback.
7. **RF card:** QPF4507 pin-level reference, C5 TX/RX switching, 2.4 GHz unused port termination, fast detector selection and power monitors.
8. **Power tree:** input range and connectors; host and radio current budgets, sequencing, brownout, back-power prevention, thermals.
9. **Mechanical:** board outline, mounting holes, FPC mating-cycle rating, 100 mm cable orientation, keyed connector placement and labeling.
10. **Manufacturing:** PCB stack-up and DFM, JST-GH footprint variants, component sourcing and assembly house, schematic/KiCad libraries, license choice.
