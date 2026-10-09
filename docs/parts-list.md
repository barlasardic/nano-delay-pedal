# Parts list — prototype revision C

See [BOM.txt](../BOM.txt) for the plain-text manufacturing BOM.

Select parts by both electrical value and the footprint dimensions. Resistors use compact DIN0204 bodies (about 3.6 × 1.6 mm), 7.62 mm lead spacing, 1% tolerance, at least 0.125 W. Standard larger quarter-watt bodies may need a footprint/layout change.

| References | Qty | Value / part | Package |
|---|---:|---|---|
| A1 | 1 | Arduino_Nano_v3.x | Arduino_Nano_EdgeUSB (project-local Delay library) |
| C1 | 1 | 47u 25V | CP_Radial_D6.3mm_P2.50mm |
| C2 | 1 | 330n | C_Disc_D3.0mm_W1.6mm_P2.50mm |
| C3, C6 | 2 | 47u 16V | CP_Radial_D6.3mm_P2.50mm |
| C4, C8, C9, C10, C14, C16, C17, C19, C20 | 9 | 100n | C_Disc_D3.0mm_W1.6mm_P2.50mm |
| C5 | 1 | 100n film | C_Rect_L7.2mm_W2.5mm_P5.00mm |
| C7, C11, C12, C18 | 4 | 10n | C_Disc_D3.0mm_W1.6mm_P2.50mm |
| C13 | 1 | 1u film | C_Rect_L7.2mm_W3.5mm_P5.00mm |
| C15 | 1 | 100u 16V | CP_Radial_D6.3mm_P2.50mm |
| D1 | 1 | 1N4007 | D_DO-41_SOD81_P10.16mm_Horizontal |
| D2 | 1 | 1N4148 | D_DO-35_SOD27_P7.62mm_Horizontal |
| D3, D4 | 2 | BAT85 | D_DO-35_SOD27_P7.62mm_Horizontal |
| J1 | 1 | 9V IN: 1=+ 2=GND | PinHeader_1x02_P2.54mm_Vertical |
| J2 | 1 | INPUT: 1=TIP 2=SLEEVE | PinHeader_1x02_P2.54mm_Vertical |
| J3 | 1 | OUTPUT: 1=TIP 2=SLEEVE | PinHeader_1x02_P2.54mm_Vertical |
| J4 | 1 | TIME B10k | PinHeader_1x03_P2.54mm_Vertical |
| J5 | 1 | FEEDBACK B10k | PinHeader_1x03_P2.54mm_Vertical |
| J6 | 1 | MIX B10k | PinHeader_1x03_P2.54mm_Vertical |
| J7 | 1 | MOMENTARY NO | PinHeader_1x02_P2.54mm_Vertical |
| J8 | 1 | LED: 1=A 2=K | PinHeader_1x02_P2.54mm_Vertical |
| JP1 | 1 | POWER LINK: remove for USB | PinHeader_1x02_P2.54mm_Vertical |
| K1 | 1 | G5V-2-H1 DC5 | Relay_DPDT_Omron_G5V-2 |
| Q1 | 1 | 2N3904 | TO-92_Inline_Wide |
| R1, R11, R13 | 3 | 1M | R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal |
| R2, R3, R14, R15, R16, R17 | 6 | 10k | R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal |
| R4, R5, R6, R8 | 4 | 2k2 | R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal |
| R7, R18 | 2 | 1k | R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal |
| R9 | 1 | 100k | R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal |
| R10 | 1 | 22 | R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal |
| R12 | 1 | 100 | R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal |
| U1 | 1 | LM7805_TO220 | TO-220-3_Vertical |
| U3 | 1 | MCP6002-I/P | DIP-8_W7.62mm_Socket |
| U4 | 1 | 23LC1024-I/P | DIP-8_W7.62mm_Socket |

## Extra hardware

- Two 1×15 female sockets, 2.54 mm pitch, for the Nano.
- Two DIP-8 sockets with 7.62 mm row spacing, if not already counted with IC purchases.
- Three B10k linear pots, knobs and mounting hardware.
- Two mono 6.35 mm panel jacks.
- One insulated panel DC jack compatible with the chosen regulated 9 V center-negative adapter.
- One normally-open momentary footswitch.
- One LED; its series 1 kΩ resistor is already R18 on the PCB.
- One 2.54 mm shunt for JP1.
- Four M2.5 insulating standoffs and screws; verify actual head/washer clearance.
- Hookup wire, short shielded audio cable, and secure header plugs if desired.

Capacitor packages: 100 nF/10 nF small ceramics use a 2.50 mm pitch and 3 mm disc body. Electrolytics use 2.50 mm pitch, 6.3 mm diameter; verify height. C5 uses a 7.2 × 2.5 mm, 5 mm pitch film body. C13 uses a 7.2 × 3.5 mm, 5 mm pitch film body; select a 1 µF part that fits (typically a compact 50 V polyester part). Do not substitute a wider film capacitor without checking neighboring parts.

The exact purchasing BOM remains to be confirmed against available supplier part numbers. Values and package envelopes are specified here; this is not a supplier-validated cart.
