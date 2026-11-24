# ROV-GND-P4 Rev A — 100 × 100 mm square placement

Status: **mechanical concept based on component body widths, not a routed PCB or final connector-courtyard validation**.

![Dimensioned 100 mm top-view placement](rov-gnd-p4-placement-rev-a.svg)

The SVG physical board outline is 100 mm × 100 mm when printed at 100% with no page scaling. The drawing is a footprint *study*, not a manufactured PCB or an authoritative KiCad 3D model.

## Why 100 mm

The **second USB-C (FS/JTAG) has been removed**. The P4 retains native USB FS/JTAG, with GPIO24 D− and GPIO25 D+ routed **only to the east 3×13 GPIO header**, to be used with an optional separate USB-C adapter.

West edge required: **one USB-C HS OTG + six JST-GH connectors**.

| Item | Dimension |
|---|---:|
| One USB-C body (reference width) | ~8.94 mm |
| Two JST-GH 6P bodies, 10.75 mm each | 21.50 mm |
| Four JST-GH 4P bodies, 8.25 mm each | 33.00 mm |
| **Total connector bodies** | **63.44 mm** |
| Six gaps × 3 mm | 18.00 mm |
| Two end margins | 18.56 mm |
| **Total west edge** | **100.00 mm** |

Note: measurements are **representative connector body widths**, not guaranteed courtyard/latch/cable-boot spacing. Before freezing the outline, verify exact chosen connector drawings and actual cable insertion/removal access.

### West edge coordinates, north to south

Measured from the northwest edge corner, in mm:

| Connector | Start y | End y |
|---|---:|---:|
| USB-C HS/OTG | 9.28 | 18.22 |
| UART0 GH 6P | 21.22 | 31.97 |
| UART1 GH 4P | 34.97 | 43.22 |
| UART2 GH 4P | 46.22 | 54.47 |
| External I2C GH 4P | 57.47 | 65.72 |
| SPI GH 6P | 68.72 | 79.47 |
| Ethernet GH 4P | 82.47 | 90.72 |

Nominal margins at top and bottom: **9.28 mm each**.

## Compass layout

### North: Camera
15-pin, 1.0 mm pitch Raspberry Pi-style CSI-2 FPC, camera cable exits north. P4 sits close to north-center to minimize MIPI trace length.

### West: External connectors
From north to south: **USB-C HS OTG**, UART0 (6-pin GH), UART1 (4-pin GH), UART2 (4-pin GH), I²C (4-pin GH), SPI (6-pin GH), Ethernet (4-pin GH). The Ethernet PHY and magnetics remain *on the board*. **No onboard USB FS/JTAG port and no SPI-EXT JST-GH**.

### East: GPIO / servo
3×13 at 2.54 mm pitch, logical GND / 5 V / signal rows 3–13, two power-only rows at top. Signals are GPIO4, GPIO5, GPIO6, GPIO13, GPIO32, GPIO33, GPIO22, GPIO23, GPIO54, GPIO25/USB FS D+ and GPIO24/USB FS D−. The optional FS USB-C breakout must use D± plus GND, fit required Type-C CC pull-downs and ESD, and **must not connect cable VBUS directly to the header 5 V supply rail**.

### South: Radio
15-pin SDIO 1.0 mm-pitch FPC centered on south, plus separate JST radio power and main 5 V input. Radio flex contains SDIO + interleaved GND only; target cable length is 100 mm.

### Interior
P4 near CSI north-center; microSD accessible on northeast; ethernet magnetics southwest; regulators and radio power protection southeast. All are provisional component zones.

## Validation before a final board outline

1. Check actual USB-C/JST-GH **courtyards, cable overmolds and latch clearances**, not just body width.
2. Verify 3×13 header polarity, row/pin numbering, 5 V distribution and servo-cable access.
3. Ensure microSD extraction, both FPC latches, and four mounting screws remain accessible.
4. Audit P4 pin map, boot straps, USB HS OTG power design, UART0 DTR/RTS, Ethernet and camera routing.
5. Keep native USB FS/JTAG D± on header short for optional external adapter; GPIO is 3.3 V-only.
6. Increase board to 105 or 110 mm if connector/latch keepouts or routing require it.

**Mechanical baseline:** 100 × 100 mm square; awaiting footprint and cable fit verification.
