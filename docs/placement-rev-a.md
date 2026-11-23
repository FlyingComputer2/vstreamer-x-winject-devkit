# ROV-GND-P4 Rev A — square mechanical placement draft

Status: **mechanical concept / not routed / dimensions provisional**.

## Board form factor

- **Initial envelope:** 120 × 120 mm square (including connectors; exact outline and mounting keep-outs TBD).
- **Top view convention:** north = camera edge, west = communication connectors, east = GPIO/servo header, south = replaceable radio interface.
- **PCB:** initial four-layer stack-up with uninterrupted reference ground plane below the high-speed routes. Footprints and edge-clearance/mechanical courtyard checks are required before choosing a smaller board.
- **Mounting holes:** four near corners, exact center offsets and screw diameter TBD once the connector courtyards are fixed.

![Conceptual component placement](rov-gnd-p4-placement-rev-a.svg)

## Edge assignments

### North — camera

- A 15-contact, 1.0 mm-pitch Raspberry Pi-style **CSI-2 camera FPC** near the center of the north edge.
- Face its cable exit toward north; check connector top/bottom-contact orientation and latch accessibility.
- The P4 should sit immediately south of the CSI interface, with a clean and short MIPI route and no switching power components nearby.
- Keep camera I²C separate from the exposed I²C bus.

### West — GH and USB

Recommended **north to south** arrangement:

| Position | Connection | Contact count |
|---:|---|---:|
| 1 | USB-C **HS/OTG** (VStreamer host/device) | USB-C |
| 2 | USB-C **FS/JTAG** (native peripheral; *not* a USB-to-UART bridge) | USB-C |
| 3 | **UART0 / FLASH** (3V3 ref, GND, TX0, RX0, DTR, RTS) | JST-GH 6 |
| 4 | **UART1 / MSP DisplayPort** | JST-GH 4 |
| 5 | **UART2 / CRSF-S.Port** | JST-GH 4 |
| 6 | **I²C / external** | JST-GH 4 |
| 7 | **SPI** (3V3, GND, CLK, MOSI, MISO, CS0) | JST-GH 6 |
| 8 | **Ethernet / 100BASE-TX cable** (magnetics on PCB) | JST-GH 4 |

- All west-facing connectors should have cable exit and finger/latch clearance.
- Ethernet PHY, magnetics and cable-side ESD near the southwest corner; do not run its twisted-pair copper beneath MIPI.
- Keep USB ESD/protection close to the connector; route USB differential pairs to P4 over a continuous ground plane.
- **There is no SPI-EXT connector**: CS1–CS5 are GPIO/servo-header signals only.

### East — 3×13 servo-style GPIO

Vertical header centered on the east edge. Logical row order remains:

- Row 1: GND / 3V3 / 3V3
- Row 2: GND / 5V / 5V
- Rows 3–13: GND / 5V / GPIO4, GPIO5, GPIO6, GPIO13, GPIO32, GPIO33, GPIO22, GPIO23, GPIO54, GPIO25/D+, GPIO24/D− (respectively).

Place the **signal column closest to the P4/core**, 5 V in the middle and GND at the exterior. This preserves logical GND/5V/signal when reading inward from the east edge. Final silk orientation and physical pin-1 numbering will be explicitly defined with the chosen footprint. The 5 V rail is shared and must be current-limited/protected; signal pins remain 3.3 V-only.

### South — replaceable radio

- Centrally placed 15-contact / 1.0 mm-pitch FPC labelled **WINJECT SDIO**, latching and exiting south.
- Immediately adjacent **radio 5 V power JST**, separate from the FPC. Confirm the connector part number/current rating.
- **Main 5 V input** at southwest south edge, separate from radio power output (input connector/voltage range TBD).
- SDIO cable is 100 mm and carries only DAT0–DAT3, CMD, CLK and interleaved ground; no differential transceiver, UART or power.
- The radio PCB has its own flashing, debug and BOOT/RESET facilities.

## Interior component zoning

| Zone | Placement rationale |
|---|---|
| North-center | P4, flash, decoupling and clock near center, below CSI FPC; MIPI shortest path |
| Center-west | USB protection and optional data-path isolation for GPIO24/25 |
| Southwest | Ethernet PHY, magnetics, ESD and twisted pair connector; keep noise away from CSI |
| East-upper corner | MicroSD socket with accessible card insertion away from the GPIO pin field |
| Southeast-center | Power supply, radio power switch/current monitor; isolate switching currents from MIPI |
| Center-south | SDIO interface source damping footprints, contiguous grounded routing to radio FPC |

## Mechanical and electrical review gates

1. Verify real USB-C and JST-GH **body/courtyard sizes and cable exit directions**; the left stack is a packing constraint.
2. Ensure nothing blocks the microSD card extraction path, camera FPC latch, radio FPC latch or 3×13 servo plugs.
3. Reserve mounting hole keep-outs without crossing high-speed routes.
4. Use 90 Ω differential USB, 100 Ω differential MIPI, and 50 Ω single-ended SDIO routing targets subject to the chosen PCB stack-up. Follow Espressif's current P4 hardware guidelines.
5. Keep CSI and SDIO edge labels large and distinctive. Although the connectors have the same mechanics, **their electrical pinouts differ**.
6. This draft does **not** approve the earlier conversation GPIO map. Pin capabilities and boot-strap conflicts must be audited before schematic sign-off.
7. Review connector accessibility with a physical 1:1 printed or 3D mechanical model before shrinking below 120 × 120 mm.
