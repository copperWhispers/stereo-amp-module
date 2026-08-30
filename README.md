# STEREO AMP MODULE

An open-source stereo class-D amplifier board built around the TI TPA3116D2. Full KiCad source, gerbers, and a BOM.
Link to TPA3116D2 Part: https://www.ti.com/lit/ds/symlink/tpa3118d2.pdf?ts=1788008862639&ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252FTPA3118D2
This is an interesting part with many different configurations which can handle 4 ohms and 8 ohms Stereo Speakers at various Power ratings:
<img width="778" height="404" alt="image" src="https://github.com/user-attachments/assets/cd6d183b-aab9-4626-83b4-ef15c580c287" />


**Here is an image of the PCB I designed for it:**

<img width="1103" height="753" alt="image" src="https://github.com/user-attachments/assets/93ebcd17-d26f-4aaf-aa94-0d7bf5273be4" />


---

## What it is

| | |
|---|---|
| **Amplifier** | TPA3116D2DADR — class-D, stereo, BTL |
| **Configuration** | Master mode, 26 dB gain, differential inputs |
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

As the board is single layer as I am building it using a UV, photosensitive film and copper clad board method, thus external connections have to be made via wires, I have placed pads on the layout to make the soldering process easier:

**Wire link between these pads:**

<img width="604" height="546" alt="image" src="https://github.com/user-attachments/assets/21bb1652-2472-457f-aa00-73e5f42c8a68" />

**Solder bridge across these legs:**

<img width="406" height="182" alt="image" src="https://github.com/user-attachments/assets/232b2e7b-0955-4ed9-b916-7c04dbd50d00" />



---

## Extras

**GAIN**

Going in, I expected to set the gain to whatever the maths called for, pick a resistor ratio, get that gain. The TPA3116D2 doesn't work that way. It offers four fixed settings: 20, 26, 32 and 36 dB selected by a voltage divider on the GAIN/SLV pin. The setting latches at power-up and can't be changed while running. 
I will thus be modulating the volume via a later preamp module that will also serve as an inputs mixer, I will probably add a couple of other fun features on there too, and it will allow me to protect the input of the audio amplifier by ensuring that signals get attenuating to the right scale before going in.

This board ended up at **26 dB** (20 kΩ to GND, 100 kΩ to GVDD). Which if we use the equation  dB = 20 * Log(Gain)  then rearrange for Gain ->  Gain = 10^(dB/20) we get Gain = 19.95.

However as I show in the video there are many other settings that you can configure this gain to and this can be achieved simply by changing the GAIN/SLV resistor values:

<img width="684" height="74" alt="image" src="https://github.com/user-attachments/assets/db5be88f-ebab-43f1-aea7-5f6ad9c4c478" />


**BTL vs SLV**





**Speakers Safety:** 

the speakers are never at risk. On a 12 V rail the maximum output is about 18 W into 4 Ω, and the speakers are rated 20 W. The supply voltage is the protection, the amplifier physically cannot deliver enough power to damage them, whatever you do with the volume knob.

---

## Video

[https://youtu.be/IWhPQ7ZQIiU](https://youtu.be/IWhPQ7ZQliU?si=qsnklUcxvsyva8gy)

## Licence

**CERN-OHL-S v2** for hardware, **CC BY-SA 4.0** for documentation — see `LICENSE`.

Build it, modify it, sell it. If you distribute a modified design, that design has to be open too.

---

Built one? Open an issue with a photo — I'd like to see it.

New board every month: [copperWhispers](https://copperwhispers.com)
