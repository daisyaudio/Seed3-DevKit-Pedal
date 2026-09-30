# Seed3 Pedal Dev Kit

<img width="100%" height="auto" alt="Seed3 Pedal Dev Kit" src="https://github.com/user-attachments/assets/2152c641-f47e-42f1-9ed4-89b2e0deee4f" />

## An Effects Pedal Platform for the Daisy Seed3

The Seed3 Pedal Dev Kit provides everything you need to design and prototype your next Daisy-based effects pedal on a single, purpose-built board. Create powerful DSP effects — massive reverbs, delay networks, complex loopers, spectral processors, and granular engines the likes of which have yet to grace the pedal world.

With industry-standard components and circuitry onboard, moving a Daisy design into production has never been easier. On top of stereo I/O and a host of electromechanical controls, the Dev Kit includes DIN MIDI I/O, a TRS expression input, a microSD socket, and a USB-C port. Take your DSP pedals to the next level with Daisy.

---

## Contents

- [Features](#features)
- [Specifications](#specifications)
- [Getting Started](#getting-started)
- [Hardware Reference](#hardware-reference)
- [Resources & Support](#resources--support)
- [Open-Source Hardware](#open-source-hardware)
- [License](#license)

---

## Features

| Category | Details |
| --- | --- |
| **Audio** | Stereo audio input and output |
| **MIDI** | DIN MIDI In, Out, and Thru |
| **Expression** | TRS expression pedal input |
| **USB** | USB-C port for programming, power, and USB MIDI/serial |
| **Storage** | microSD slot for firmware updates, samples, presets, configuration, and more |
| **Potentiometers** | 6 × 10 kΩ linear (B-taper) |
| **Footswitches** | 2 × footswitches |
| **Buttons** | 2 × tactile switches |
| **Toggle Switches** | 3 × toggle switches (1 × ON-OFF-ON, 2 × ON-ON) |
| **LEDs** | 1 × RGB LED, 1 × red LED |
| **Power** | 9 V DC barrel jack, center negative (standard pedal supply) |

## Specifications

| Parameter | Value |
| --- | --- |
| Processor module | Daisy Seed3 |
| Supply voltage | 9 V DC, center negative |
| Current draw | firmware dependent |
| Reverse polarity protection | Yes |
| Audio codec / sample rate | TAC5242 / up to 32-bit, 192kHz |
| Input impedance | 1MΩ |
| Output impedance | 100Ω |
| Bypass | Buffered |
| Board dimensions | 197 mm × 100 mm |
| Expression input | TRS |
| MIDI connectors | 3 × 5-pin DIN (In, Out, Thru) |

> [!WARNING]
> Use a **center-negative** 9 V DC supply.

## Getting Started

### 1. Set up the toolchain

Install the Daisy toolchain and clone the libraries by following the setup guide at [docs.daisy.audio](https://docs.daisy.audio).

- [libDaisy](https://github.com/electro-smith/libDaisy) — hardware abstraction library
- [DaisySP](https://github.com/electro-smith/DaisySP) — DSP library

### 2. Build the template
 
A ready-to-go starting project for the Pedal Dev Kit lives in libDaisy at
[`examples/devkits/Pedal-DevKit-Template`](https://github.com/daisyaudio/libDaisy/tree/master/examples/devkits/Pedal-DevKit-Template).
 
```bash
git clone --recurse-submodules https://github.com/daisyaudio/libDaisy
cd libDaisy
make
cd examples/devkits/Pedal-DevKit-Template
make
```
 
Copy the template folder to start your own effect, then edit the audio callback. Inputs are `in[0][i]` (left) and `in[1][i]` (right), and outputs are `out[0][i]` and `out[1][i]` (see [Audio](#audio)).

### 3. Flash the Seed3

Connect the Dev Kit to your computer over USB-C, put the Seed3 into bootloader mode, then flash:

```bash
make program-dfu
```

### 4. Power up

Disconnect USB (or leave it connected), connect a 9 V center-negative supply, and plug in your instrument and amp.

## Hardware Reference

### Pinout

<img width="100%" height="auto" alt="seed3-pedal-dev-kit-dark" src="https://github.com/user-attachments/assets/061ae26e-2a6c-463a-b9ea-c3694a19000f" />


### Audio

| Jack | Silkscreen | Signal | Audio Callback |
| --- | --- | --- | --- |
| Input Left | IN LEFT | `AUDIO_IN_L` | `in[0][i]` |
| Input Right | IN RIGHT | `AUDIO_IN_R` | `in[1][i]` |
| Output Left | OUT LEFT | `AUDIO_OUT_L` | `out[0][i]` |
| Output Right | OUT RIGHT | `AUDIO_OUT_R` | `out[1][i]` |

### Controls

| Control | Board Ref | Signal | Seed3 Pin | Notes |
| --- | --- | --- | --- | --- |
| Potentiometer 1 | VR1 | `POT_1` | D15 | 10 kΩ B-taper |
| Potentiometer 2 | VR2 | `POT_2` | D16 | 10 kΩ B-taper |
| Potentiometer 3 | VR3 | `POT_3` | D17 | 10 kΩ B-taper |
| Potentiometer 4 | VR4 | `POT_4` | D18 | 10 kΩ B-taper |
| Potentiometer 5 | VR5 | `POT_5` | D19 | 10 kΩ B-taper |
| Potentiometer 6 | VR6 | `POT_6` | D20 | 10 kΩ B-taper |
| Expression input | EXPRESSION IN | `ADC_EXPRESSION` | D21 | TRS |
| Footswitch 1 | FSW_1 | `GPIO_FSW_1` | D10 | |
| Footswitch 2 | FSW_2 | `GPIO_FSW_2` | D23 | |
| Tactile switch 1 | SW3 | `TAC_SW1` | D0 | |
| Tactile switch 2 | SW4 | `TAC_SW2` | D28 | |
| Toggle switch 1 | SW1 | `TOG_2_A` / `TOG_2_B` | D8 / D7 | ON-OFF-ON (3-position, two pins) |
| Toggle switch 2 | SW2 | `TOG_1` | D9 | ON-ON |
| Toggle switch 3 | SW6 | `TOG_3` | D22 | ON-ON |

### LEDs

| LED | Board Ref | Signal | Seed3 Pin |
| --- | --- | --- | --- |
| Red LED | LED1 | `LED_MONO_1` | D24 |
| RGB LED — Red | LED2 | `LED_R` | D25 |
| RGB LED — Green | LED2 | `LED_G` | D27 |
| RGB LED — Blue | LED2 | `LED_B` | D26 |

### MIDI

| Jack | Signal | Seed3 Pin | Direction |
| --- | --- | --- | --- |
| MIDI Out | `MIDI_TX` | D13 | Out |
| MIDI In | `MIDI_RX` | D14 | In |
| MIDI Thru | `MIDI_RX` | D14 | Mirrors MIDI In |

### microSD (SDMMC, 4-bit)

| Signal | Seed3 Pin |
| --- | --- |
| `SDMMC_CK` | D6 |
| `SDMMC_CMD` | D5 |
| `SDMMC_D0` | D4 |
| `SDMMC_D1` | D3 |
| `SDMMC_D2` | D2 |
| `SDMMC_D3` | D1 |

### USB-C

| Signal | Seed3 Pin |
| --- | --- |
| `USB_OTG_HS_N` | D29 |
| `USB_OTG_HS_P` | D30 |


## Resources & Support

- **Documentation:** [docs.daisy.audio](https://docs.daisy.audio)
- **Community Forum:** [community.daisy.audio](https://community.daisy.audio)
- **Store:** [daisy.audio](https://daisy.audio)
- **Issues:** Report bugs or hardware errata via this repository's [Issues](../../issues) tab.

---

## Open-Source Hardware

<img width="256px" height="auto" alt="Open Source Hardware logo" src="https://github.com/user-attachments/assets/f9264744-3509-4cf0-9f4a-981cb05eb38e" />

The Pedal Dev Kit is open-source hardware and carries the essential circuitry needed to use Daisy in effects pedal applications. Use the BOM, schematics, and board files to kickstart your own pedal designs.

## License

The Seed3 Pedal Dev Kit is released under the permissive **MIT License**. This means that:

- Designers are free to use, study, modify, share, and distribute the hardware designs and products based on them.
- Designers may use any and all provided designs in closed-source commercial products.

See [LICENSE](LICENSE) for the full text.

© 2026 Qu-Bit Electronix, Inc. (dba Daisy)
