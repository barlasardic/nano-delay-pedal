# Revision C design review - 2026-10-10

The review covered schematic connectivity, component pin assignments, power/bypass wiring, component courtyards, routing, board edges, mounting holes and silkscreen. The netlist is unchanged from revision B. No additional circuit-connection error was found in this review. This does not establish that the unbuilt circuit or planned firmware works.

## Changes made

- Board width increased from 50.80 to 55.88 mm while retaining 88.90 mm height; four 2 mm corner radii added. This provides room for the regulator capacitors and component labels within a compact custom enclosure concept.
- The input/output headers remain at the side edges, and Nano USB still points right.
- Regulator bypass capacitors moved alongside U1. The Nano supply bypass and final ADC capacitor now sit above their Nano pins. The flyback diode spans the two relay coil connections nearby.
- Power, ground and relay-coil routes increased from 0.25 to 0.50 mm. Signal routes remain 0.25 mm; clearance remains 0.20 mm.
- Added all 57 reference/value labels on B.Silkscreen, 17 values on F.Silkscreen, wiring abbreviations and connector polarity. Back text is mirrored correctly for underside viewing.
- Filled ground zones regenerated. C14 ground pad uses a solid zone connection to resolve an isolated thermal-relief island; routed ground copper also connects that pad.

The table reports straight-line pad separation, not measured signal quality. The four bypass/filter connections listed first are directly routed over 3.81 mm in this board.

| Connection | Revision B | Revision C |
|---|---:|---:|
| Regulator input bypass (C2.1 to U1.1) | 7.79 mm | 3.81 mm |
| Regulator output bypass (C4.1 to U1.3) | 15.79 mm | 3.81 mm |
| Nano supply bypass (C17.1 to A1.27) | 20.08 mm | 3.81 mm |
| Final ADC shunt capacitor (C18.1 to A1.19) | 69.12 mm | 3.81 mm |
| Input coupling to op-amp (C5.2 to U3.3) | 8.91 mm | 5.08 mm |

## Checks performed

KiCad 9 ERC and PCB DRC, including schematic parity, report zero violations, zero unconnected pads and zero footprint-association errors. The board contains 57 electrical footprints plus four mechanical holes. A separate netlist comparison confirms the circuit connections did not change. BOM values and footprints are checked against the PCB. Copper widths, board outline, hole positions and label coverage are checked from the saved design.

The relay pin mapping and unpowered bypass path, its flyback diode polarity, regulator I/G/O pins, op-amp follower connections, ADC bias and clamps, SRAM supply/pin mapping, and JP1 USB isolation were reviewed against the schematic and the sources below. The SRAM provides adequate storage for the 300 ms target; firmware timing and audio behavior still need implementation and testing.

## Limits before ordering an assembled production run

This remains a prototype. Confirm supplier packages and pinouts, actual Nano/USB geometry, electrolytic height, socket height and enclosure/panel hardware before ordering. The enclosure drawing is a proposal for a custom housing; BOSS publishes external dimensions, which do not determine a compatible internal cavity. Use the PCB mounting template only for PCB hole locations, not as an unverified enclosure drilling plan.

Bench work still includes power-rail checks, regulator temperature at actual load, relay switching, ADC/PWM timing, noise, clipping, PWM residual and anti-alias performance. The passive filters intentionally limit the audio bandwidth; no commercial BOSS sound-quality claim is made. The MCP6002 data sheet recommends very close supply bypassing; the socketed through-hole layout uses a short 3.81 mm VDD-to-C16 connection and needs bench evaluation.

## Sources checked

- [BOSS DS-1 compact external dimensions](https://in.boss.info/products/ds-1/): 73 × 129 × 59 mm overall reference.
- [TI LM7805 family data sheet, pin configuration and section 10](https://www.ti.com/lit/ds/symlink/lm340.pdf): regulator pinout, bypass placement and supply routing.
- [Microchip MCP6001/2/4 data sheet, sections 4.3 and 4.4](https://ww1.microchip.com/downloads/en/DeviceDoc/MCP6001-1R-1U-2-4-1-MHz-Low-Power-Op-Amp-DS20001733L.pdf): op-amp supply, bypass and capacitive-load considerations.
- [Microchip 23LC1024 data sheet](https://ww1.microchip.com/downloads/en/DeviceDoc/20005142C.pdf): memory pinout, 2.5-5.5 V operation and SPI interface.
- [Omron G5V-2 / G5V-2-H1 data sheet](https://omronfs.omron.com/en_US/ecb/products/pdf/en-g5v_2.pdf): relay connections, package and H1 coil selection.
