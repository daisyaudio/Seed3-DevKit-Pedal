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

This Dev Kit is open-source hardware, built to the [Open Source Hardware Definition](https://www.oshwa.org/definition/) published by the Open Source Hardware Association (OSHWA). The schematics, PCB layouts, bill of materials, and KiCad source files are published so that you can study, modify, manufacture, and sell your own designs based on them.

## License

The hardware design files in this repository are licensed under the **CERN Open Hardware Licence Version 2 – Permissive** ([CERN-OHL-P-2.0](https://ohwr.org/cern_ohl_p_v2.pdf)).

Subject to the terms of that licence, you may:

- Use, study, copy, modify, and distribute these designs and any products made from them.
- Incorporate these designs, in whole or in part, into closed-source and commercial products.

When you redistribute these designs or products made from them, you must:

- Retain all copyright, licence, and other notices contained in the source files.
- Add a notice to any modified source stating that you modified it, with the date and a brief description of the change.
- Ensure that recipients of any product made from these designs have access to the applicable notices.

These designs are provided "as is", without warranty of any kind, express or implied. See [LICENSE](https://github.com/user-attachments/files/32935889/LICENSE.txt) for the full licence text, including the disclaimer of warranty and limitation of liability.

Firmware and software, including [libDaisy](https://github.com/electro-smith/libDaisy) and [DaisySP](https://github.com/electro-smith/DaisySP), are licensed separately under the terms included in their respective repositories.

SPDX-License-Identifier: CERN-OHL-P-2.0

### Trademarks

DAISY® is a trademark of Qu-Bit Electronix, Inc., registered in the United States. CERN-OHL-P-2.0 grants a license to the copyright and related rights in these designs. It does not grant any right or license to use the DAISY name, logos, product names, or other trademarks of Qu-Bit Electronix, Inc.

Products, derivative designs, and related materials made from these designs may not:

- Use the DAISY name or logo, or any confusingly similar name or mark, in a product name, model number, brand, domain name, or marketing material.
- Reproduce the DAISY name or logo on a PCB silkscreen, front panel, enclosure, packaging, or documentation, except where needed to keep the required licence notices.
- State or imply that the product is made, endorsed, sponsored, certified, or supported by Qu-Bit Electronix, Inc.

You may make truthful, factual statements about compatibility or origin, such as "based on the Seed3 Pedal Dev Kit design" or "compatible with the Daisy Seed3", provided the statement does not suggest affiliation or endorsement. If you redistribute or sell products derived from these designs, remove the DAISY name and logo from the silkscreen and other artwork before manufacture.

For trademark licensing or permission requests, contact Qu-Bit Electronix, Inc. through [daisy.audio/pages/support](https://daisy.audio/pages/support).

© 2026 Qu-Bit Electronix, Inc. (dba Daisy)
