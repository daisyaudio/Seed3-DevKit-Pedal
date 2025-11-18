# PCB changelog

## Rev3

- Updated rev text (on bottom side) to Rev3
- Marked R10, and R19 as populate, and R9 as DNP to swap the mid-rail voltage to ~+4V5
- Updated R43 (RED LED Resistor) to 1K (previously 2K)
- Updated C8 and C28 to 100pF (previously 1nF) to fix tone-suck/low-pass filtering of guitars, etc.
- Updated R75 and R12 to 33K (previously 24K) to improve pass-through level in the audio I/O circuits
- Renamed toggle/button switches so that they're designators match the order of the layout.
- Updated copyright to "(C) Daisy 2025"
- Added silkscreen indications for each Daisy pin to the corresponding electro-mechanical element.

### TODO

- [x] Need to replace all PIP footprints
- [x] Replace QR code

### Footprints to use

| Type | New footprint |
| ---- | ------------- |
| 1/4" jack | Connector_audio:Jack_6.35mm_Neutrik_NMJ6HCD2_Horizontal |
| Pots | parts-for-kicad:9MM_SNAP_-IN_POT_SILK |
| Toggle (on-on) | Electrosmith-Interface:TOGGLE_ON-ON |
| Toggle (on-off-on) | Electrosmith-Interface:TOGGLE_ON_OFF_ON |

## Rev2

Lots of little changes, and removed relays, etc.

## Rev1

Initial Rev
