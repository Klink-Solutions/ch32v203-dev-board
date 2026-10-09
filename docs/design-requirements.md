# Design Requirements

**How to use:** 
* Every requirement has a ID 
* Priority is **Must / Should / May**; helps decide priorities when space gets tight.
* Verification says how each requirement will be proven.

## Revision history

| Rev | Date | Author |
|-----|------|--------|
| 0.1 | 2026-10-07 | Lucas K.| 
| 0.2 | 2026-10-09 | Lucas K. (edited with Claude) |

## Electrical

| ID | Requirement | Priority | Rationale | Verification |
|----|-------------|----------|-----------|--------------|
| REQ-E-001 | The board shall be powered from USB-C 5 V with 5.1 kOhm pull-downs on both CC pins so it works with C-to-C cables and chargers. | Must | Without CC resistors many USB-C supplies output nothing. | Power from an A-to-C and a C-to-C source; measure rail. |
| REQ-E-002 | When USB-powered (up to 40 °C ambient), the 3.3 V rail shall supply 250 mA total (MCU plus external loads) while the battery is charging, and 350 mA when it is not, within the MCU's supply range. | Must | Useful dev board; external sensors and modules. Limited by the 0.5 A resettable fuse (0.41 A hold at 40 °C), so total USB draw stays at or below 350 mA. | Load the 3V3 pin at 250 mA while charging and at 350 mA with the battery full or absent, 10 min each; the fuse must not trip; measure regulator temperature. |
| REQ-E-003 | When battery-powered, the 3.3 V rail shall stay at 3.3 V down to a cell voltage of about 3.6 V, and below that shall follow the cell while staying inside the MCU's supply range down to 2.7 V. | Should | Predictable behavior as the cell discharges; a 3.3 V LDO cannot regulate below its dropout voltage. | Discharge test with a bench supply. |
| REQ-E-004 | The 5V pin only carries USB 5 V (after input protection) only while USB is connected. | Must | No boost converter fits necessary. | Measure 5V pin on battery with and without USB. | 
| REQ-E-005 | The board shall safely charge a single-cell Li-ion/LiPo from USB using a constant-current profile. | Should | Safe, standard Li-ion charging; the cell can stay connected. | Charge a protected cell; read current and voltage. | 
| REQ-E-006 | Default charge current shall be 100 mA (0.2C for a 500 mAh cell), set by a single resistor (10 kOhm). | Should | Gentle on a 500 mAh cell, and keeps total USB draw within the fuse limit (see REQ-E-002). | Measure charge current during the constant-current phase. |
| REQ-E-007 | When USB is connected the system shall run from USB and the battery shall not supply the load; when USB is absent the system shall run from the battery (power-path behavior). | Should | Battery does not drain while on USB, and charging is independent of load. | Measure battery current with USB connected and load active. | 
| REQ-E-008 | Protect against reversed battery polarity | Should | Incosistent JST wiring | Connect a reversed bench supply (current-limited); check the board still works after. | 
| REQ-E-009 | A resettable fuse shall protect the USB 5 V input: hold current 0.5 A (0.41 A at 40 °C), above the 350 mA worst-case normal draw, and trip current 1 A. | Must | "Idiot-proof". | Short the 3V3 pin; confirm the fuse trips and resets after the short is removed. |
| REQ-E-010 | The board shall be unharmed and shall operate normally when: no battery is connected; a battery is plugged while USB is connected/ USB is plugged while on battery. | Should | "idiot-proof". | Perform each case. | 
| REQ-E-011 | The board shall have an easily accessible reset and BOOT button shall be provided. | Must | Recoverable programming with no external tools. | Enter the bootloader and flash firmware.| 
| REQ-E-012 | The board shall have a user-controllable status / charge-status LED, using dedicated cip pins. | Should | Visible feedback. | Visual check while charging.| 
| REQ-E-013 | ESD protection should be provided on the USB data lines. | May | USB-C port may come in contact with unwanted statics turing normal operation.| Inspect schematic. | 
| REQ-E-014 | The board shall not require an external crystal. Footprint and load-capacitor pads for a crystal on the HSE pins shall be provided, left unpopulated by default. | Should | Allows cheaper config & proves config for other projects | Test USB and general operation with and without the crystal fitted. | 
| REQ-E-015 | Battery voltage should be readable by firmware through a high-value resistor divider on a internal chip pin . | May | Battery level indication. | Read the ADC value against a multimeter . | 
| REQ-E-016 | The development board must be based on the CH32V203C8T6 MCU. | Must | Proves system capabilities for future integrated project| Run sample code of exsisting project. | 
| REQ-E-017 | When battery-powered, the 3.3 V rail shall supply up to 500 mA, using a cell rated for at least 1C discharge. | Should | The fuse is not in the battery path; the limits are the LDO and the cell. | Load the 3V3 pin at 500 mA from a charged cell; check rail voltage and regulator temperature. |

## Mechanical / Form factor

| ID | Requirement | Priority | Rationale | Verification |
|----|-------------|----------|-----------|--------------|
| REQ-M-001 | The board outline shall match the XIAO footprint( 21 x 17.8 mm.) | Must | Plug and play capability | check official datasheet. | 
| REQ-M-002 | 14 edge pads at 2.54 mm pitch shall match the XIAO pin positions and work with or without pin headers. | Must | Compatibility; Castellated holes are supported by JLCPCB. | Compare with datasheet.| 
| REQ-M-003 | All parts shall be hand-solderable (gull-wing or leaded packages only), passives 0603 or larger. Listed exceptions only: USB-C connector. | Must | Keeps project at least plausible without a reflow oven | BOM package review. | 
| REQ-M-004 | Parts should be contained to the top side of the board. | Should | Allows easy breadboard use and soldering. | Test-fit on a breadboard. | 
| REQ-M-005 | Pin names shall be printed on the silkscreen, bottom side if space is short. | Should | Easy use without a pinout chart. | | 

## Interfaces / Pinout

| ID | Requirement | Priority | Rationale | Verification |
|----|-------------|----------|-----------|--------------|
| REQ-I-001 | The 14 edge pins shall carry D0-D10, 3V3, 5V and GND in the standard XIAO order. | Must | Drop-in XIAO pinout. | Compare with the official XIAO pinout. | 
| REQ-I-002 | USB-C shall provide both USB device function and programming (bootloader). | Must | Single cable for power, data and programming. | Flash and power over USB. | 
| REQ-I-003 | Every chip pin not on the edge pins, plus the debug pins, reset and BOOT, shall be available as test pads. | Should | allows expanded capabilities if needed. |  | 
| REQ-I-004 | Two plated through-holes shall allow an optional 2.0mm JST battery connector to be fitted on the back, with polarity printed on the silkscreen. | May | Easier than soldering to pads; omitted when used on a breadboard. | | 

## Manufacturing / Cost

| ID | Requirement | Priority | Rationale | Verification |
|----|-------------|----------|-----------|--------------|
| REQ-C-001 | The PCB shall be a JLCPCB standard 4-layer board and obey its DRC. | Must | Fixed fab and layer count to control cost. | Run the DRC with JLCPCB rules. | 
| REQ-C-002 | Five complete boards (PCB, parts, shipping) shall cost no more than USD 60. | Must | Project budget. | Keep Full BOM with JLC quote and shipping cost overhead | 
| REQ-C-003 | All parts should be in stock at the chosen supplier/suppliers when the order is placed. | should | Avoids backlog. | | 


## Open questions
- **Crystal and USB:** Double check whether the CH32V203 USB works without an external crystal. If it needs one, REQ-E-014 must change.
- **MCU minimum supply:** Confirm in the CH32V203 datasheet that it runs down to 2.7 V (REQ-E-003).
- **Reverse battery:** the power-path FET isolates the system from a reversed cell, but the charger and the battery divider sit on the raw battery net. A series P-FET at the battery connector would meet REQ-E-008.
- **Battery connector:** the BOM's J2 is a 2.5 mm JST XH; REQ-I-004 calls for 2.0 mm. Change the part to a 2.0 mm one.
- **LDO copper:** the TLV75733P only reaches its 100.8 °C/W rating on TI's evaluation board; keep generous copper on its pins (350 mA is about 0.4-0.5 W).
- **Charge-current resistor:** confirm 10 kOhm against the MCP73831 datasheet (I = 1000 / R in kOhm).
- **More power later:** a fuse with about 1.1 A hold would allow a 500 mA load plus 250 mA charging, but needs a Schottky rated 1 A or more at D1 and a supply that can deliver it; costs size and money.
