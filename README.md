# STEREO AMP MODULE

An open-source stereo class-D amplifier board built around the TI TPA3116D2. Full KiCad source, gerbers, and a BOM.

Built to answer a specific question: what does it actually take to drive a pair of £10 speakers properly?

<img width="1103" height="753" alt="image" src="https://github.com/user-attachments/assets/93ebcd17-d26f-4aaf-aa94-0d7bf5273be4" />


---

## What it is

| | |
|---|---|
| **Amplifier** | TPA3116D2DADR — class-D, stereo, BTL |
| **Configuration** | Master mode, 26 dB gain, differential inputs |
| **Input impedance** | 30 kΩ |
| **Supply** | 4.5–20 V (built and tested at 12 V) Could go to 26V with different caps.|
| **Output** | ~18 W into 4 Ω, limited by the 12 V rail |
| **Idle current** | ~50 mA at 12 V |
| **Assembly** | Hand-solderable — 0603 passives, HTSSOP-32|


An interactive BOM is included at [`docs/ibom.html`](docs/ibom.html) — open it in a browser and it highlights each part on the board as you work through the build.

---

## Build it

BOM: Open docs/ibom.html in a browser.

**Boards:** order `production/gerbers.zip` from JLCPCB or PCBWay, or build the boards yourself.

**Schematic:** [`docs/schematic.pdf`](docs/schematic.pdf) · **Board:** [`docs/pcb.pdf`](docs/pcb.pdf)

## Errata — v1 needs two wire links

The board as fabbed requires two manual connections:

**Wire link between these pads:**
<img width="604" height="546" alt="image" src="https://github.com/user-attachments/assets/21bb1652-2472-457f-aa00-73e5f42c8a68" />

**Solder bridge across these legs:**
<img width="406" height="182" alt="image" src="https://github.com/user-attachments/assets/232b2e7b-0955-4ed9-b916-7c04dbd50d00" />



---

## What I got wrong

**I assumed gain would be a design variable. It isn't.**

Going in, I expected to set the gain to whatever the maths called for, pick a resistor ratio, get that gain. The TPA3116D2 doesn't work that way. It offers four fixed settings: 20, 26, 32 and 36 dB selected by a voltage divider on the GAIN/SLV pin. The setting latches at power-up and can't be changed while running.

This board ended up at **26 dB** (20 kΩ to GND, 100 kΩ to GVDD). The maths in the video called for roughly half that, which is where the ×10 versus ×20 discrepancy comes from.

**What it means in practice:** the groovebox has to run at a lower volume than it otherwise would, or the input overdrives the amplifier and the signal distorts. Within the lower part of the groovebox's volume range it works well — plenty loud, clean, no audible distortion.

**What it doesn't mean:** the speakers are never at risk. On a 12 V rail the maximum output is about 18 W into 4 Ω, and the speakers are rated 20 W. The supply voltage is the protection, the amplifier physically cannot deliver enough power to damage them, whatever you do with the volume knob.

**The fix**, and the subject of the next board: a proper input attenuator, so the source level can be matched to the amplifier's fixed gain.

---

## Video

https://youtube.com/@Kylian_Delaplassette

## Licence

**CERN-OHL-S v2** for hardware, **CC BY-SA 4.0** for documentation — see `LICENSE`.

Build it, modify it, sell it. If you distribute a modified design, that design has to be open too.

---

Built one? Open an issue with a photo — I'd like to see it.

New board every month: [copperWhispers](LANDING_PAGE_URL)
