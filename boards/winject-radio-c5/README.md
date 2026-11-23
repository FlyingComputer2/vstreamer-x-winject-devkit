# WINJECT-RADIO-C5

Replaceable ESP32-C5 radio prototype.

- ESP32-C5 and QPF4507 5 GHz FEM.
- Fast RF detector, final IC/location TBD.
- Independent C5 UART flashing/debug and BOOT/RESET.
- SDIO slave on 15-pin / 1.0 mm pitch camera-type FPC, **SDIO+GND only**.
- Separate JST power, local regulation, optional RF baseline/bypass path.
- Board remains functional for standalone radio testing; RF measurements use properly rated load and measurement equipment.

See [radio SDIO interface](../../docs/radio-sdio-interface.md).
