# Arduino Nano delay pedal — prototype revision A

A fully routed, two-layer PCB draft for a mono delay with a **300 ms target**. This is an unbuilt prototype. Firmware is not included, and audio performance has not been measured. Validate the circuit on a breadboard or prototype before ordering a production batch.

Open **delay-pedal.kicad_pro** in KiCad 9 or later. This revision updates the root project files. Earlier revisions remain available in Git history.

## Design choices

- Board: **50.8 × 88.9 mm**, 1.6 mm thick, two copper layers, conventional 1 oz copper. This is a compact-pedal layout; fit in a particular BOSS enclosure has not been established.
- Four 2.7 mm mounting holes for M2.5 hardware, centered 3.81 mm from each corner. Use insulating spacers and account for screw-head clearance.
- Through-hole parts, socketed DIP ICs, and a Nano on two 15-pin female sockets. Match the Nano's USB end to the footprint marking.
- Wired enclosure controls: three B10k linear pots, two mono 6.35 mm jacks, normally-open momentary footswitch, LED, and external DC jack. Board connections use 2.54 mm headers; solder wires directly or use secure mating plugs with strain relief.
- External **23LC1024-I/P SPI SRAM** keeps the Nano as the audio processor and provides delay storage. Do not substitute the lower-voltage 23A1024.
- **MCP6002-I/P** replaces the TL072 and runs from filtered 5 V. Do not fit a TL072 in U3.
- **G5V-2-H1 DC5** relay gives bypass when de-energized. Use the 5 V H1 sensitivity version; a standard G5V-2 DC5 draws more current and requires a driver/power review.
- Input and output filters use two cascaded 2.2 kΩ/10 nF RC sections, approximately 2.7 kHz combined -3 dB bandwidth. The target is a deliberately lo-fi sound, not commercial BOSS audio quality.
- Both sides have ground pours. Signal tracks are 0.25 mm with 0.20 mm minimum clearance; vias are 0.65 mm with 0.30 mm drill.

The live schematic was rebuilt into labeled functional sections. The input stays centered near 2.5 V through the ADC filter; D2's cathode connects to the relay's positive supply. Relay contact numbering now matches the chosen physical part.

## Wiring

Pin 1 is the square pad, viewed from the component side. Headers may face different directions: use pad numbers, not a single left-to-right assumption. Pot pin assignments below describe function; verify the pot's terminal numbering with its datasheet or a meter.

| Header | Pin 1 | Pin 2 | Pin 3 |
|---|---|---|---|
| J1 — 9 V power | Positive 9 V | Ground | — |
| J2 — input jack | Tip | Sleeve/ground | — |
| J3 — output jack | Tip | Sleeve/ground | — |
| J4 — time B10k | CCW end/ground | Wiper | CW end/+5 V logic |
| J5 — feedback B10k | CCW end/ground | Wiper | CW end/+5 V logic |
| J6 — mix B10k | CCW end/ground | Wiper | CW end/+5 V logic |
| J7 — footswitch | Switch terminal | Other terminal/ground | — |
| J8 — LED | Anode | Cathode/ground | — |
| JP1 — removable power link | Regulator +5 V | Logic +5 V | — |

For a center-negative pedal adapter, wire the external DC jack's **center to J1 pin 2** and **outer contact to J1 pin 1**. Identify switching contacts separately; do not rely on their physical order. D1 provides series reverse-polarity protection. Use a regulated 9 V adapter; the board is not intended for mains wiring.

Keep input/audio wiring short and away from the Nano, memory bus, and relay coil. Use shielded audio cable where practical. Mount the mechanical controls to the enclosure and provide strain relief at the board.

## USB power jumper

**Remove the JP1 shunt before connecting USB.** With JP1 removed, the Nano's USB supply powers the logic, memory, and analog section through the Nano's +5 V pin; it is separated from the board regulator. This avoids tying the regulator directly to the USB-powered Nano rail, including on clones with different power circuits.

For programming: turn off/unplug the 9 V adapter, remove JP1, then connect USB. The relay will remain in bypass without its 9 V-powered regulator. After programming, disconnect USB, refit JP1, and reconnect 9 V. Fit JP1 for normal pedal use. The Nano VIN pin is intentionally unused.

## Nano pin assignment / firmware contract

| Nano pin | Function |
|---|---|
| A0 | Biased audio input |
| A1 | Time pot |
| A2 | Feedback pot |
| A3 | Mix pot |
| D2 | Active-low footswitch; external pull-up and 100 nF filter |
| D5 / OC0B | 8-bit PWM audio output |
| D7 | LED, high = on |
| D8 | Relay driver, high = effect engaged |
| D10 | SRAM chip select, active low |
| D11 | SRAM MOSI |
| D12 | SRAM MISO |
| D13 | SRAM clock |

A 16 ksample/s, 8-bit circular delay buffer needs 4,800 bytes for 300 ms. The SRAM supplies storage; it does not perform audio processing. A 62.5 kHz fast-PWM carrier on D5 is the intended starting point. Firmware must configure the timers directly; ordinary analogWrite defaults are unsuitable, and reconfiguring Timer0 affects Arduino timing functions.

Firmware must initialize SPI/SRAM, center PWM near half-scale before engaging the relay, schedule ADC audio and control readings without audible gaps, and implement delay, feedback limiting, and dry/wet mixing. Input bias and PWM midpoint must agree. ADC rate/resolution, timer interference, memory timing, clipping, and switching pops all require testing. External memory makes the requested storage feasible; it does not prove real-time firmware performance.

## Assembly and first checks

Fit resistors, diodes, sockets, capacitors and headers first. Check diode bands, electrolytic polarity and the 2N3904 E/B/C pinout. U3 and U4 are different ICs despite sharing DIP-8 sockets. The Nano uses sockets, not a bare ATmega328 chip.

Before inserting the Nano and DIP ICs, check for shorts, then apply current-limited 9 V and confirm approximately 5 V at 5V_REG. Fit JP1 and check the logic and analog rails, plus approximately 2.5 V at VB_2V5 after its capacitor settles. Measure the regulator temperature under the actual load. There is space to assess a small TO-220 heatsink, but its mechanical fit has not been validated.

With firmware installed, verify unpowered bypass, input/ADC bias, PWM carrier and filtered output using an oscilloscope, then measure delay time, noise, clipping, feedback stability and relay switching behavior. Check with the actual enclosure, jacks and pot bodies before committing mounting holes in the enclosure.

## Files and verification

- KiCad schematic, board and project: editable design sources.
- ../delay-pedal.pdf: circuit drawing.
- assembly-top.pdf: component positions and references, including references printed on the reverse side.
- pcb-preview.png: top view of the actual routed board.
- parts-list.md: grouped board parts and off-board hardware.
- board-check.txt and schematic-check.txt: KiCad checks from the delivered revision.
- ../manufacturing/: Gerber and Excellon files for the **unvalidated prototype**; these are not evidence of functional testing.

The delivered design is checked for electrical-rule violations, copper clearances, unconnected pads and schematic/PCB agreement. These checks do not establish audio performance or physical compatibility with every supplier's part. Confirm body dimensions, lead pitch and pinout when purchasing parts, especially film capacitors, relay, transistor and Nano clones.

## Primary references

- [Microchip 23LC1024](https://www.microchip.com/en-us/product/23LC1024) and [pinout/protocol datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/20005142C.pdf).
- [Microchip MCP6002](https://www.microchip.com/en-us/product/MCP6002).
- [Omron G5V-2 / H1 datasheet](https://omronfs.omron.com/en_US/ecb/products/pdf/en-g5v_2.pdf).
- [Arduino Nano specifications](https://store.arduino.cc/arduino-nano).
