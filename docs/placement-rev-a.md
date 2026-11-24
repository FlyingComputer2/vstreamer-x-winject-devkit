# ROV-GND-P4 Rev A — mixed-orientation GH, physically scaled 80 × 80 mm study

**Status:** revised footprint/body placement study; **NOT a released PCB**. West edge now mixes top-entry (standing) and side-entry (lying) JST GH parts. In the diagram, 1 SVG unit = 1 mm physical: print at **100%, no scaling** for 80 mm board outline.

![Measured-dimension board diagram](rov-gnd-p4-placement-rev-a.svg)

## Connector selection

| Port | Proposed exact part | Orientation | Plastic body along west edge | Footprint envelope along edge used for placement |
|---|---|---|---:|---:|
| USB HS OTG | `GCT USB4085-GF-A` | Right-angle, cable W | 8.94 mm | 9.40 mm |
| UART0 · 6P | `JST SM06B-GHS-TB` | Right-angle, cable W | 10.75 mm | 11.25 mm |
| UART1 · 4P | `JST BM04B-GHS-TBT` | Top-entry, pin axis E–W, cable +Z | 4.05 mm | 6.60 mm |
| UART2 · 4P | `JST BM04B-GHS-TBT` | Top-entry, pin axis E–W, cable +Z | 4.05 mm | 6.60 mm |
| I²C · 4P | `JST BM04B-GHS-TBT` | Top-entry, pin axis E–W, cable +Z | 4.05 mm | 6.60 mm |
| SPI · 6P | `JST BM06B-GHS-TBT` | Top-entry, pin axis E–W, cable +Z | 4.05 mm | 6.10 mm |
| ETH · 4P | `JST SM04B-GHS-TB` | Right-angle, cable W | 8.25 mm | 8.75 mm |

**Sources:** JST GH drawing: https://www.marutsu.co.jp/contents/shop/marutsu/datasheet/BM04B-GHS-TBT%28LF%29%28SN%29.pdf ; vertical GH KiCad examples: https://wiki.adom.inc/adom/bm04b-ghs-tbt/files/bm04b-ghs-tbt.kicad_mod and https://wiki.adom.inc/adom/bm06b-ghs-tbt/files/bm06b-ghs-tbt.kicad_mod ; USB-C GCT drawing: https://www.mouser.com/datasheet/2/837/USB4085-2888250.pdf .

**Important distinction:** the *plastic body width* of a rotated top-entry GH is only **4.05 mm**, but its SMT pads and courtyard extend farther. The drawing therefore uses **6.60 mm** top-entry 4P and **6.10 mm** top-entry 6P envelopes based on footprint examples, and **estimated** 0.5 mm additional clearance for the two right-angle GH bodies and the USB body. Reconfirm all footprints from the definitive KiCad library before fixing connector spacing. Cabling above the board also needs full 3D checks.

### Actual physical sizing and boundary conditions

- Board: **80 × 80 mm**, top view, millimeter-scale SVG.
- Mixed GH + USB West envelope lengths total: **55.30 mm**.
- Six envelope gaps × 2.00 mm = **12.00 mm**.
- Combined west occupancy = **67.30 mm**, remaining **12.70 mm**, placed with **6.35 mm** nominal end margins.
- JST top-entry part body is 8.25 mm (4P) or 10.75 mm (6P) **inboard**; entry opening points up away from board (+Z). It is not electrically different from right-angle JST GH.
- GCT USB4085-GF-A USB Type-C body shown **8.94 × 9.17 mm** (USB2.0 480 Mbps HS OTG).
- ESP32-P4 QFN104 package is **10 × 10 mm**.
- Two 15P 1.0 mm-pitch Molex `0522071533` reference FFC/FPC bodies shown **19.7 × 6.9 mm**, with CSI north and radio SDIO south. **Confirm actual choice and FFC contact side/orientation.** Source: https://s3.amazonaws.com/snapeda/datasheet/522071533_Molex.pdf .
- Molex microSD `5033981892` socket shown **13.1 × 14.05 mm**. Source: https://www.molex.com/content/dam/molex/molex-dot-com/en_us/pdf/product-reference-guides/987651-8263.pdf .
- 3×13 servo header has **2.54 mm** pin pitch, with first-to-last row center span **30.48 mm**. GND/5V/signal ordering is unchanged; USB FS/JTAG D−/D+ GPIO24/GPIO25 stay only on this header, no onboard FS connector.
- South main power and radio power are drawn as **candidate 4-pin GH side-entry bodies**; exact connector and current rating remain unresolved. Their labels `VIN*` and `R-PWR*` indicate placeholders.
- PHY, magnetics and buck regulator zones are **functional placement placeholders, not validated manufacturer dimensions**.
- Corner holes are preliminary and **not** mechanically approved.

## Cable clearance and PCB constraints

1. Top-entry GH cables emerge perpendicular to the PCB. Check mating housing height, insulation bend radius and removal access, including adjacent standing plugs and the enclosure roof.
2. Side-entry GH cables project outward to the west. Check actual connector rotation, latch access and JST mating plug orientation.
3. A printed housing size alone is insufficient for final PCB DRC. Verify **KiCad courtyard and SMT mounting pads** for selected exact variants. The dashed envelopes are indicative until chosen footprint files are imported.
4. UART0 6P and Ethernet 4P deliberately remain side-entry for external harness access. UART1/2, external I²C and SPI switch to top-entry (standing).
5. Ensure PCB mount screws, camera FPC latch, radio SDIO latch, microSD extraction, 3×13 header and USB shell are accessible with cables installed.
6. Native USB FS/JTAG from GPIO24/25 requires a separate external USB-C adapter with GND/CC/ESD and no direct cable VBUS-to-servo 5V backfeed.
7. P4 GPIO/boot-strap routing, USB-C HS host VBUS and Ethernet PHY routing are not yet verified.

## Recommendation

**Use 80 × 80 mm as a promising new mechanical target**, **not** a committed PCB outline. Re-run physical placement using vendor STEP models / KiCad exact footprints and full mated-harness models; grow to 85–90 mm if any west-edge courtyard, routing or mounting clearance fails.
