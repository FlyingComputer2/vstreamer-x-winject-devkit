# ROV-GND-P4 Rev A — footprint-based square placement

Status: **mechanical sizing study, not routed or manufacturing-ready**.

## Working board dimension: **110 × 110 mm**

The previous 120 × 120 mm outline was an unverified envelope. Reducing to **110 × 110 mm** is realistic as a starting point using the eight required connector bodies on the west edge. The west edge dictates the minimum practical size; the 3×13 servo header does not.

![Nominally dimensioned 110 × 110 mm top-view placement](rov-gnd-p4-placement-rev-a.svg)

The SVG declares its physical dimensions in millimeters. **Print at 100% (without "fit to page")** to obtain a nominal 1:1 board outline. Connector bodies are measured/approximated by representative part dimensions, not yet final KiCad courtyards.

### West edge connector packing (north to south)

| Connection | Quantity | Candidate width per body along board edge |
|---|---:|---:|
| USB-C 2.0 connector (HS OTG, FS/JTAG) | 2 | ~8.94 mm |
| JST-GH 6P (UART0 / FLASH; SPI) | 2 | ~10.75 mm |
| JST-GH 4P (UART1, UART2, external I²C, Ethernet) | 4 | ~8.25 mm |
| **Total connector bodies** | **8** | **72.38 mm** |
| 7 × 3 mm between successive connector bodies | | 21.0 mm |
| Top and bottom unoccupied edge length | | 16.62 mm |
| **Total west edge** | | **110.0 mm** |

Representative candidate footprints: JST `SM06B-GHS-TB` 6P (10.75 mm), `SM04B-GHS-TB` 4P (8.25 mm), and USB-C receptacle width approximately 8.94 mm. Actual final mating widths and USB connector keep-outs may change. This is a **minimum connector-body packing test**, not a mechanical signoff.

Exact west body start/stop locations relative to the northwest corner (mm, top-down):

| Connector | Start y | End y |
|---|---:|---:|
| USB-C HS/OTG | 6.00 | 14.94 |
| USB-C FS/JTAG | 17.94 | 26.88 |
| UART0 JST-GH 6P | 29.88 | 40.63 |
| UART1 JST-GH 4P | 43.63 | 51.88 |
| UART2 JST-GH 4P | 54.88 | 63.13 |
| External I²C JST-GH 4P | 66.13 | 74.38 |
| SPI JST-GH 6P | 77.38 | 88.13 |
| Ethernet JST-GH 4P | 91.13 | 99.38 |

These eight connectors require approximately 10.62 mm at the south end and 6 mm at the north end for leftover board edge. **3 mm gaps may be too tight for a finger-operated GH latch or thick USB cable overmolds**; check harness mating with actual 3D models and a physical print before layout release. If hand access is difficult, increase outline or move one connector rather than compressing below safe clearances.

### Other edges

- **North:** 15-pin 1.0 mm Raspberry Pi-style CSI-2 camera FPC centered and exiting north. Place P4 near north-center to keep high-speed MIPI short.
- **East:** 3×13 pin 2.54 mm pitch servo-style header, GND / 5V / SIGNAL logically when reading from outer board edge inward. The centers of its 13 rows span **30.48 mm**; body/cable clearance extends beyond.
- **South:** centered 15-pin 1.0 mm radio SDIO FPC exiting south, with separate radio power JST and separate main 5 V supply connector (power connector selections TBD).
- **Inside:** place Ethernet PHY/magnetics near southwest, microSD near northeast, P4 near CSI, and power supply toward southeast. These are zoning recommendations not final routed footprints.

## Mounting and routing

- Show four **provisional** corner mounting holes. Actual hole diameter, keep-out and screw-head clearance must be selected after connector footprints are placed.
- Protect 3.3 V-only GPIO signal pins from 5 V electrical input; the GND/5V/SIGNAL ordering is standard for servo-style connections.
- The SPI-EXT JST-GH connector was removed. CS1–CS5 are provided on the GPIO header instead.
- Native USB FS/JTAG and UART0 are separate interfaces, with two USB-C ports and separate 6-pin UART0 auto-flash JST-GH.
- Keep CSI and SDIO FPC ports clearly labeled despite their identical cable form factor.
- Keep the SDIO cable target at 100 mm, single-ended, with interleaved GND and power via separate connector.
- Final P4 GPIO map/strapping/power circuitry is **not signed off**. See [GPIO map](gpio-map-draft.md) and [open hardware items](open-items.md).

## Decision

**Use 110 × 110 mm as the Rev A mechanical baseline**. At 100 × 100 mm, the eight west connectors need narrower gaps, tighter end margins, or moving a connector to another edge. A 110 mm square should be mechanically plausible but should not be interpreted as the minimum manufacturable dimensions without 3D courtyard and cable-access verification.
