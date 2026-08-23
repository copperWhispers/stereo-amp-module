# STEREO AMP MODULE

An open-source stereo class-D amplifier board built around the TI TPA3116D2. Full KiCad source, gerbers, and a BOM that fills your cart in one click.

Built to answer a specific question: what does it actually take to drive a pair of £10 speakers properly?

`[HERO PHOTO — board on the bench, speakers behind]`

---

## What it is

| | |
|---|---|
| **Amplifier** | TPA3116D2DADR — class-D, stereo, BTL |
| **Configuration** | Master mode, 26 dB gain, differential inputs |
| **Input impedance** | 30 kΩ |
| **Supply** | 4.5–26 V (built and tested at 12 V) |
| **Output** | ~18 W into 4 Ω, limited by the 12 V rail |
| **Idle current** | ~50 mA at 12 V |
| **Board** | 2 layers, 63 footprints |
| **Assembly** | Hand-solderable — 0603 passives, HTSSOP-32 PowerPAD |

The board carries 18 through-hole test pads and dedicated test points. It's designed to be measured, not just used — you can scope the switching node, the filter output and the supply rail without soldering to a leg.

An interactive BOM is included at [`docs/ibom.html`](docs/ibom.html) — open it in a browser and it highlights each part on the board as you work through the build.

---

## Build it

BOM: Open docs/ibom.html in a browser.

**Boards:** order `production/gerbers.zip` from JLCPCB or PCBWay. 2-layer, 1.6 mm, HASL is fine. No controlled impedance needed.

**Schematic:** [`docs/schematic.pdf`](docs/schematic.pdf) · **Board:** [`docs/pcb.pdf`](docs/pcb.pdf)

The only awkward part is U1 — the TPA3116D2 is HTSSOP-32 with an exposed PowerPAD underneath. The pins are straightforward at 0.65 mm pitch, but the pad needs proper thermal contact or the chip shuts down under load. Everything else is 0603 and through-hole.

---

## What I got wrong

**I assumed gain would be a design variable. It isn't.**

Going in, I expected to set the gain to whatever the maths called for — pick a resistor ratio, get that gain. The TPA3116D2 doesn't work that way. It offers four fixed settings — 20, 26, 32 and 36 dB — selected by a voltage divider on the GAIN/SLV pin. You pick one of four and live with it. The setting latches at power-up and can't be changed while running.

This board ended up at **26 dB** (20 kΩ to GND, 100 kΩ to GVDD). The maths in the video called for roughly half that, which is where the ×10 versus ×20 discrepancy comes from.

**What it means in practice:** the groovebox has to run at a lower volume than it otherwise would, or the input overdrives the amplifier and the signal distorts. Within the lower part of the groovebox's volume range it works well — plenty loud, clean, no audible distortion. It's a usability limitation, not a functional one.

**What it doesn't mean:** the speakers are never at risk. On a 12 V rail the maximum output is about 18 W into 4 Ω, and the speakers are rated 20 W. The supply voltage is the protection — the amplifier physically cannot deliver enough power to damage them, whatever you do with the volume knob.

**The fix**, and the subject of the next board: a proper input attenuator, so the source level can be matched to the amplifier's fixed gain instead of the other way round.

---

## Design notes

### Why 12 V and not 21 V

The chip will do roughly 2×50 W into 4 Ω at 21 V. The speakers here are rated 20 W. Running the rail lower is what keeps the amplifier from being able to destroy the load — the supply voltage is the protection.

### The output filter

Four 11 µH inductors and their filter caps reconstruct the audio from the switching output. 3.3 Ω + 1 nF snubbers across the output nodes damp the switching edges and keep radiated EMI down. The topology follows TI's recommendation from the datasheet.

---

## Specs and measurements

| Parameter | Calculated | Datasheet | Measured |
|---|---|---|---|
| Output power, 4 Ω | ~18 W | — | not yet measured at clipping |
| Gain | 26 dB | 25–27 dB | — |
| Idle current | — | 20–35 mA | ~50 mA |
| Source level used | — | — | ~2 Vpk typical, 4 Vpk peak |

Measured on a 12 V rail driving 20 W 4 Ω speakers. If you measure yours, open an issue — I'll add it with credit.

---

## Repo layout

```
├── hardware/          KiCad project — schematic, PCB
├── production/        Gerbers + drill, fab-ready zip
├── docs/              Schematic PDF, board PDF, interactive BOM, images
└── bom/               BOM.csv, cart links
```

---

## Video

https://youtube.com/@Kylian_Delaplassette

## Licence

**CERN-OHL-S v2** for hardware, **CC BY-SA 4.0** for documentation — see `LICENSE`.

Build it, modify it, sell it. If you distribute a modified design, that design has to be open too.

---

Built one? Open an issue with a photo — I'd like to see it.

New board every month: [copperWhispers](LANDING_PAGE_URL)
