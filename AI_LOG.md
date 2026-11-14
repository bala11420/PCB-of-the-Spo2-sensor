# AI log

I used an AI assistant (Claude) during this project. This log lists where, what it produced, what it got wrong, and what I did.
Entries marked [confirm] need me to check or fill in my own decision before publishing.

| # | What I asked | What the AI produced | What I kept, changed or rejected |
|---|---|---|---|
| 1 | Which sensor options exist for SpO2 on a Raspberry Pi | A list: MAX30102, MAX32664, AFE4490 | Too generic. I needed to pick one and give reasons myself. I chose the AFE4490 [confirm my reasons] |
| 2 | Whether an ESP32 with an SD card could do the job | Yes, with limits (clock, power cuts, display, network) | The idea was mine. I did not pursue it further [confirm] |
| 3 | Review of an AI-made design package (netlist, BOM, checks) against the datasheet | The pin table matched. The package's own check report only compared the design with its own data file | I checked against the PDF myself. Findings: the 3.3 V LED-supply rejection used the wrong headroom value for that case, crystal capacitors were placeholders, no ESD protection on probe lines, reset pulse too short, pull-up divider with the Pi's default pull-downs, probe type not fixed |
| 4 | A wiring sheet for the schematic | A PNG with a pin-by-pin wiring plan | Used as a reference only. My schematic differs (see `hardware/wiring_sheet/README.md`) |
| 5 | Clock part I picked in EasyEDA | It is an active oscillator, not a passive crystal. Wiring for either choice | My schematic uses a passive crystal with 18 pF capacitors [confirm] |
| 6 | Whether the AFE4490 detects low signal and noise itself | Only electrical faults, in hardware. Low signal and noise need software | I will not claim more than that |
| 7 | Review of my schematic PDF | Issues listed in `docs/design_review.md` | Open items remain. [confirm which I fixed] |
| 8 | Probe options | Nellcor DS-100A family as a candidate, with warnings | Nothing bought, pinout unverified |
| 9 | Bring-up script for MAX30102 | `software/max30102_bringup/max30102_bringup.py` | Never run on real hardware. Values must be checked against the datasheet |

## Things the AI got wrong

- It assumed a sensor photo in an email was my own board. It was an example from the other person.
- An AI-made wiring drawing labelled the chip MAX30100 while the chosen chip was MAX30102.
- The AI package's 3.3 V headroom arithmetic for the LED supply used the wrong headroom value for that case.
- It first suggested building around the ESP32 even though the requirements named the Raspberry Pi.

## One suggestion I rejected

[Write one AI suggestion you rejected and why, in your own words.]
