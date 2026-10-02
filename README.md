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
- [Making Your Own Project](#making-your-own-project)
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
| **USB** | USB-C port connected to the Seed3's USB High Speed peripheral, for USB features in your firmware |
| **Storage** | microSD slot for firmware updates, samples, presets, configuration, and more |
| **Potentiometers** | 6 × 10kΩ linear (B-taper) |
| **Footswitches** | 2 × footswitches |
| **Buttons** | 2 × tactile switches |
| **Toggle Switches** | 3 × toggle switches (1 × ON-OFF-ON, 2 × ON-ON) |
| **LEDs** | 1 × RGB LED, 1 × red LED |
| **Power** | 9V DC barrel jack, center negative (standard pedal supply) |

## Specifications

| Parameter | Value |
| --- | --- |
| Processor module | Daisy Seed3 |
| Supply voltage | 9V DC, center negative |
| Current draw | Firmware dependent |
| Reverse polarity protection | Yes |
| Audio codec / sample rate | TAC5242 / up to 32-bit, 192kHz |
| Input impedance | 1MΩ |
| Output impedance | 100Ω |
| Bypass | None (audio always passes through the Seed3) |
| Board dimensions | 197mm × 100mm |
| Expression input | TRS |
| MIDI connectors | 3 × 5-pin DIN (In, Out, Thru) |

> [!WARNING]
> Use a **center-negative** 9V DC supply.

## Getting Started

### 1. Install the toolchain

Follow the Daisy [C++ Getting Started guide](https://docs.daisy.audio/tutorials/cpp-dev-env/). It installs the toolchain and clones [DaisyExamples](https://github.com/daisyaudio/DaisyExamples), which includes [libDaisy](https://github.com/daisyaudio/libDaisy) (hardware library) and [DaisySP](https://github.com/daisyaudio/DaisySP) (DSP library).

### 2. Update libDaisy

The Pedal Dev Kit template and board support are newer than the copy of libDaisy that DaisyExamples includes. From your `DaisyExamples` folder, update libDaisy to the latest version and rebuild it:

```bash
git submodule update --remote libDaisy
cd libDaisy
make
```

### 3. Build the template

The starting project for this kit is [`examples/devkits/Pedal-DevKit-Template`](https://github.com/daisyaudio/libDaisy/tree/master/examples/devkits/Pedal-DevKit-Template). From the `libDaisy` folder:

```bash
cd examples/devkits/Pedal-DevKit-Template
make
```

This creates `Pedal-DevKit-Template.bin` in the template's `build/` folder.

### 4. Flash the Seed3

> [!IMPORTANT]
> Program the Seed3 through the **USB-C port on the Seed3 module itself**, not the USB-C port on the Dev Kit board.

1. Connect a USB-C cable from your computer to the Seed3's USB-C port.
2. Hold **BOOT**, press and release **RESET**, then release **BOOT**.
3. From the template folder, run:

   ```bash
   make program-dfu
   ```

   Or, in the [Daisy Web Programmer](https://flash.daisy.audio/), upload the `Pedal-DevKit-Template.bin` file from the `build/` folder.

### 5. Power up and try it out

Connect a 9V center-negative supply to the barrel jack (J8) and plug in your instrument and amp. You can leave the Seed3's USB cable connected so you can watch the serial output.

> [!TIP]
> If you are experiencing USB noise from keeping the Seed3 connected to the computer, disconnect during testing.

The template:

- Passes audio from the inputs straight to the outputs.
- Blinks the red LED (LED1) and cycles the colors of the RGB LED (LED2).
- Echoes MIDI notes received on MIDI In to MIDI Out.
- Prints the state of every control over USB serial on the Seed3's USB-C port. Open any serial monitor to see it.

## Making Your Own Project

Edits only take effect on the pedal after you rebuild and reflash, so start your own project from a copy of the template.

1. Copy the `Pedal-DevKit-Template` folder into your `DaisyExamples` folder and rename it, for example `DaisyExamples/MyPedal`.
2. In the copied `Makefile`:
   - Set `TARGET` to your project name, for example `TARGET = MyPedal`.
   - Change `LIBDAISY_DIR` to point to libDaisy from the new location: `LIBDAISY_DIR = ../libDaisy`.
   - To use DaisySP, uncomment the DaisySP line and set `DAISYSP_DIR = ../DaisySP`.
3. Write your effect in `src/main.cpp`. Audio is processed in `AudioCallback()`: inputs are `in[0][i]` (left) and `in[1][i]` (right), and outputs are `out[0][i]` and `out[1][i]` (see [Audio](#audio)).
4. Rebuild and flash after every change:

   ```bash
   make
   make program-dfu
   ```

   Put the Seed3 into BOOT mode (step 4 above) before each `make program-dfu`.

> [!TIP]
> The template builds with debugging enabled (`DEBUG = 1`, `OPT = -Og`). For release builds, set `DEBUG = 0` and `OPT = -O3` in the Makefile. If your program grows too large for the Seed3's internal flash, see the [Daisy Bootloader guide](https://docs.daisy.audio/tutorials/_a7_Getting-Started-Daisy-Bootloader/).

## Hardware Reference

The **Ref** column lists each part's reference designator, as printed on the board's silkscreen.

### Pinout

<img width="100%" height="auto" alt="Seed3 Pedal Dev Kit pinout" src="https://github.com/user-attachments/assets/061ae26e-2a6c-463a-b9ea-c3694a19000f" />

A printable [Pinout PDF](https://daisy.nyc3.cdn.digitaloceanspaces.com/products/seed-3-pedal/Seed3-Pedal-Dev-Kit-Pinout.pdf) is also available.

### Audio

| Jack | Ref | Silkscreen | Signal | Audio Callback |
| --- | --- | --- | --- | --- |
| Input Left | J2 | IN LEFT | `AUDIO_IN_L` | `in[0][i]` |
| Input Right | J4 | IN RIGHT | `AUDIO_IN_R` | `in[1][i]` |
| Output Left | J3 | OUT LEFT | `AUDIO_OUT_L` | `out[0][i]` |
| Output Right | J5 | OUT RIGHT | `AUDIO_OUT_R` | `out[1][i]` |

### Controls

| Control | Ref | Signal | Seed3 Pin | Notes |
| --- | --- | --- | --- | --- |
| Potentiometer 1 | VR1 | `POT_1` | D15 | 10kΩ B-taper |
| Potentiometer 2 | VR2 | `POT_2` | D16 | 10kΩ B-taper |
| Potentiometer 3 | VR3 | `POT_3` | D17 | 10kΩ B-taper |
| Potentiometer 4 | VR4 | `POT_4` | D18 | 10kΩ B-taper |
| Potentiometer 5 | VR5 | `POT_5` | D19 | 10kΩ B-taper |
| Potentiometer 6 | VR6 | `POT_6` | D20 | 10kΩ B-taper |
| Expression input | J6 | `ADC_EXPRESSION` | D21 | TRS |
| Footswitch 1 | FSW_1 | `GPIO_FSW_1` | D10 | |
| Footswitch 2 | FSW_2 | `GPIO_FSW_2` | D23 | |
| Tactile switch 1 | SW3 | `TAC_SW1` | D0 | |
| Tactile switch 2 | SW4 | `TAC_SW2` | D28 | |
| Toggle switch 1 | SW1 | `TOG_2_A` / `TOG_2_B` | D8 / D7 | ON-OFF-ON (3-position, two pins) |
| Toggle switch 2 | SW2 | `TOG_1` | D9 | ON-ON |
| Toggle switch 3 | SW6 | `TOG_3` | D22 | ON-ON |

### LEDs

| LED | Ref | Signal | Seed3 Pin |
| --- | --- | --- | --- |
| Red LED | LED1 | `LED_MONO_1` | D24 |
| RGB LED — Red | LED2 | `LED_R` | D25 |
| RGB LED — Green | LED2 | `LED_G` | D27 |
| RGB LED — Blue | LED2 | `LED_B` | D26 |

### MIDI

| Jack | Ref | Signal | Seed3 Pin | Direction |
| --- | --- | --- | --- | --- |
| MIDI Out | J11 | `MIDI_TX` | D13 | Out |
| MIDI In | J10 | `MIDI_RX` | D14 | In |
| MIDI Thru | J1 | `MIDI_RX` | D14 | Mirrors MIDI In |

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

The Dev Kit's USB-C port connects to the Seed3's USB High Speed peripheral. It is separate from the USB-C port on the Seed3 module, which is used for programming and for the template's serial output.

| Signal | Seed3 Pin |
| --- | --- |
| `USB_OTG_HS_N` | D29 |
| `USB_OTG_HS_P` | D30 |

### Power

| Connector | Ref | Notes |
| --- | --- | --- |
| DC barrel jack | J8 | 9V DC, center negative |

## Resources & Support

- **Product Page:** [Seed3 Pedal Dev Kit](https://daisy.audio/products/seed3-development-kit-pedal)
- **Documentation:** [docs.daisy.audio](https://docs.daisy.audio/product/Seed3-Pedal-Dev-Kit/)
- **Community Forum:** [community.daisy.audio](https://community.daisy.audio)
- **Discord:** [Daisy Discord](https://discord.gg/ByHBnMtQTR)
- **Issues:** Report bugs or hardware errata via this repository's [Issues](../../issues) tab.

### Design Files

- [Schematic (PDF)](https://daisy.nyc3.cdn.digitaloceanspaces.com/products/seed-3-pedal/ES-Seed3-DevKit-Pedal-Rev3.pdf)
- [Bill of Materials (CSV)](https://daisy.nyc3.cdn.digitaloceanspaces.com/products/seed-3-pedal/Seed3-DevKit-Pedal_Rev4-bom.csv)
- [Pinout (PDF)](https://daisy.nyc3.cdn.digitaloceanspaces.com/products/seed-3-pedal/Seed3-Pedal-Dev-Kit-Pinout.pdf)
- [KiCad Design Files](https://github.com/daisyaudio/ES-Seed3-DevKit-Pedal/releases/latest)

---

## Open-Source Hardware

<img width="256px" height="auto" alt="Open Source Hardware logo" src="https://github.com/user-attachments/assets/f9264744-3509-4cf0-9f4a-981cb05eb38e" />

The Pedal Dev Kit is open-source hardware, built to the [Open Source Hardware Definition](https://www.oshwa.org/definition/) published by the Open Source Hardware Association (OSHWA). The schematics, PCB layouts, bill of materials, and KiCad source files are published so that you can study, modify, manufacture, and sell your own designs based on them.

## License

Copyright © 2026 Qu-Bit Electronix, Inc. (dba Daisy)

The hardware design files in this repository are licensed under the **CERN Open Hardware Licence Version 2 – Permissive** ([CERN-OHL-P-2.0](https://ohwr.org/cern_ohl_p_v2.pdf)).

Subject to the terms of that licence, you may:

- Use, study, copy, modify, and distribute these designs and any products made from them.
- Incorporate these designs, in whole or in part, into closed-source and commercial products.

When you redistribute these designs or products made from them, you must:

- Retain all copyright, licence, and other notices contained in the source files.
- Add a notice to any modified source stating that you modified it, with the date and a brief description of the change.
- Ensure that recipients of any product made from these designs have access to the applicable notices.

These designs are provided "as is", without warranty of any kind, express or implied. See [LICENSE](LICENSE.txt) for the full licence text, including the disclaimer of warranty and limitation of liability.

Firmware and software, including [libDaisy](https://github.com/daisyaudio/libDaisy) and [DaisySP](https://github.com/daisyaudio/DaisySP), are licensed separately under the terms included in their respective repositories.

SPDX-License-Identifier: CERN-OHL-P-2.0

### Trademarks

DAISY® is a trademark of Qu-Bit Electronix, Inc., registered in the United States. CERN-OHL-P-2.0 grants a license to the copyright and related rights in these designs. It does not grant any right or license to use the DAISY name, logos, product names, or other trademarks of Qu-Bit Electronix, Inc.

Products, derivative designs, and related materials made from these designs may not:

- Use the DAISY name or logo, or any confusingly similar name or mark, in a product name, model number, brand, domain name, or marketing material.
- Reproduce the DAISY name or logo on a PCB silkscreen, front panel, enclosure, packaging, or documentation, except where needed to keep the required licence notices.
- State or imply that the product is made, endorsed, sponsored, certified, or supported by Qu-Bit Electronix, Inc.

You may make truthful, factual statements about compatibility or origin, such as "based on the Seed3 Pedal Dev Kit design" or "compatible with the Daisy Seed3", provided the statement does not suggest affiliation or endorsement. If you redistribute or sell products derived from these designs, remove the DAISY name and logo from the silkscreen and other artwork before manufacture.

For trademark licensing or permission requests, contact Qu-Bit Electronix, Inc. through [daisy.audio/pages/support](https://daisy.audio/pages/support).
