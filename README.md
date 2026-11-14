# AFE4490 SpO2 front end for Raspberry Pi 4 (design study)

> **Status: design only.** The schematic is drawn and reviewed against the datasheet.
> Nothing here has been built, powered up or tested on hardware.

A design study for a small pulse-oximeter front end that plugs onto a Raspberry Pi 4.
The Pi talks to a Texas Instruments **AFE4490** over SPI and a few GPIO pins. The goal is a low
parts count design that can report *why* a reading cannot be trusted (for example an open LED or a
shorted photodiode) instead of always showing a number.

## What is in this repo

| Path | What it is |
|---|---|
| `hardware/schematic/` | EasyEDA schematic (PDF and PNG), drawn by me |
| `hardware/pcb/`, `hardware/3d/` | PCB layout and 3D images (add yours here, see the notes inside) |
| `hardware/wiring_sheet/` | AI-drafted wiring reference. A reference only, it differs from my schematic |
| `docs/chip_selection.md` | Why the AFE4490, compared with MAX30102 and MAX86916 |
| `docs/design_review.md` | Check of my schematic against the datasheet, with open issues |
| `docs/accuracy_and_limits.md` | What this design can and cannot claim |
| `docs/probe_options.md` | Finger-clip probe candidates (nothing bought) |
| `software/max30102_bringup/` | Bring-up script for the alternative MAX30102 route (untested, AI-written) |
| `AI_LOG.md` | Where I used AI, what it got wrong, what I changed |

## Design in brief

- **Chip:** TI AFE4490, 40-pin VQFN, 6 mm x 6 mm. Receiver supply 2.0 to 3.6 V, transmitter supply 3.0 to 5.25 V.
- **Host:** Raspberry Pi 4, SPI mode 0 plus GPIO. 5 V and 3.3 V come from the Pi header.
- **Probe:** a finger clip with a photodiode and two LEDs (not connected yet in my schematic, see issues).

### Pi header to AFE4490 (from my schematic)

| H1 pin | Pi signal | Net | AFE4490 pin |
|---|---|---|---|
| 1 | 3V3 | 3.3 V | 31, 33, 39 (receiver supplies) |
| 2 | 5V | +5V | 11, 17, 18 (transmitter supplies) |
| 6 | GND | GND | grounds |
| 11 | GPIO17 | ADC_RDY | 28 |
| 13 | GPIO27 | RESET# | 29 |
| 15 | GPIO22 | AFE_PDN# | 20 |
| 16 | GPIO23 | PD_ALM | 23 |
| 18 | GPIO24 | LED_ALM | 22 |
| 19 | GPIO10 (MOSI) | SPISIMO | 26 |
| 21 | GPIO9 (MISO) | SPISOMI | 25 |
| 22 | GPIO25 | DIAG_END | 21 |
| 23 | GPIO11 (SCLK) | SCLK | 24 |
| 24 | GPIO8 (CE0) | SPISTE | 27 |

## What the chip decides, and what software must decide

| Condition | Who detects it |
|---|---|
| Photodiode or LED open or shorted, cable shorted to ground or to the other cable | AFE4490 diagnostics (nine fault types, 11 flags, PD_ALM / LED_ALM pins) |
| Weak signal, saturation, noise, motion, finger removed from a connected probe | Software, from the raw ADC values, with thresholds set from real recordings |
| The SpO2 percentage itself | Software estimate. Accuracy depends on calibration and is not guaranteed |

See `docs/accuracy_and_limits.md`.

## Status

- [x] Schematic drawn
- [x] Checked against the TI datasheet (issues listed in `docs/design_review.md`)
- [ ] Issues fixed in the schematic
- [ ] Probe connector added
- [ ] PCB layout reviewed
- [ ] Firmware for the AFE4490
- [ ] Built and tested
- [ ] Compared against a reference oximeter

## My contribution

[Write this in your own words: what you designed yourself, what you changed after the review, what you learned.]

## AI use

I used an AI assistant for research, drafting and review. The details, including mistakes it made and
what I changed, are in `AI_LOG.md`.

## References

- TI AFE4490 product page: https://www.ti.com/product/afe4490
- TI AFE4490 datasheet (SBAS602H): https://www.ti.com/document-viewer/AFE4490/datasheet
- Distributor listing (Lion Circuits): https://www.lioncircuits.com/parts/AFE4490RHAT

The TI datasheet is not included here. Download it from the TI link above.
