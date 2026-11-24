# Third-party hardware library notices

## Espressif Systems — KiCad libraries

The `ESPRESSIF_LOCAL.pretty/ESP32-P4.kicad_mod` footprint is copied from the official Espressif KiCad libraries:

- Source repository: https://github.com/espressif/kicad-libraries
- Source file: https://github.com/espressif/kicad-libraries/blob/main/footprints/Espressif.pretty/ESP32-P4.kicad_mod
- License notice: https://github.com/espressif/kicad-libraries/blob/main/LICENSE.md

The `DEVKIT.kicad_sym` P4X pin-number and function mapping was generated from the official `ESP32-P4X` library symbol in the same repository.

Espressif's library license is **Creative Commons Attribution-ShareAlike 4.0 International**, with the library's published electronic-design exception. Read their `LICENSE.md` for the complete notice and terms. The included footprint and any modified library material retain their original license obligations; this does not choose a general license for the rest of this repository.

The Waveshare PDF is **a technical reference only** and has **not** been copied into the repository:

https://files.waveshare.com/wiki/ESP32-P4-WIFI6-DEV-KIT/ESP32-P4-WIFI6-DEV-KIT.pdf

## KiCad community footprint library

These two JST GH footprints are copied from the [official KiCad community footprint library](https://github.com/KiCad/kicad-footprints/tree/master/Connector_JST.pretty) and retain the upstream CC BY-SA 4.0 license with the KiCad electronic-design exception, as documented by the KiCad project:

- `JST_GH_BM04B-GHS-TBT_1x04-1MP_P1.25mm_Vertical.kicad_mod`
- `JST_GH_BM06B-GHS-TBT_1x06-1MP_P1.25mm_Vertical.kicad_mod`
- [KiCad library licensing](https://www.kicad.org/libraries/license/)

`ServoHeader_3x13_P2.54mm_Vertical.kicad_mod` is a new project-local mechanical footprint drafted for this project. Its pad numbering is row-major; confirm board-side pin-1 orientation before PCB release.
