# Design Requirements

**How to use:** 
* Every requirement has a ID 
* Priority is **Must / Should / May**; helps decide priorities when space gets tight.
* Verification says how each requirement will be proven.

## Revision history

| Rev | Date | Author |
|-----|------|--------|
| 0.1 | 2026-10-07 | Lucas K.| 
| 0.2 | 2026-10-09 | Lucas K. |
| 0.3 | 2026-10-11 | Lucas K. |

## Electrical

| ID | Requirement | Priority | Rationale | Verification |
|----|-------------|----------|-----------|--------------|
| REQ-E-001 | The board shall be powered from USB-C 5 V with 5.1 kOhm pull-downs on both CC pins so it works with C-to-C cables and chargers. | Must | Without CC resistors many USB-C supplies output nothing. | Power from an A-to-C and a C-to-C source; measure rail. |
| REQ-E-002 | When USB-powered (up to 40 °C ambient), the 3.3 V rail shall supply 250 mA total (MCU plus external loads) while the battery is charging, and 350 mA when it is not, within the MCU's supply range. | Must | Useful dev board; external sensors and modules. Limited by the 0.5 A resettable fuse (0.41 A hold at 40 °C), so total USB draw stays at or below 350 mA. | Load the 3V3 pin at 250 mA while charging and at 350 mA with the battery full or absent, 10 min each; the fuse must not trip; measure regulator temperature. |
| REQ-E-003 | When battery-powered, the 3.3 V rail shall stay at 3.3 V down to a cell voltage of about 3.6 V, and below that shall follow the cell while staying inside the MCU's supply range (2.4 V minimum; USB needs 3.0 V or more, and is only active when USB power is present). | Should | Predictable behavior as the cell discharges; a 3.3 V LDO cannot regulate below its dropout voltage. CH32V203 datasheet: VDD 2.4-3.6 V. | Discharge test with a bench supply. |
| REQ-E-004 | The 5V pin only carries USB 5 V (after input protection) only while USB is connected. | Must | No boost converter fits necessary. | Measure 5V pin on battery with and without USB. | 
| REQ-E-005 | The board shall safely charge a single-cell Li-ion/LiPo from USB using a constant-current profile. | Should | Safe, standard Li-ion charging; the cell can stay connected. | Charge a protected cell; read current and voltage. | 
| REQ-E-006 | Default charge current shall be 100 mA (0.2C for a 500 mAh cell), set by a single resistor (10 kOhm). | Should | Gentle on a 500 mAh cell, and keeps total USB draw within the fuse limit (see REQ-E-002). | Measure charge current during the constant-current phase. |
| REQ-E-007 | When USB is connected the system shall run from USB and the battery shall not supply the load; when USB is absent the system shall run from the battery (power-path behavior). | Should | Battery does not drain while on USB, and charging is independent of load. | Measure battery current with USB connected and load active. | 
| REQ-E-008 | Protect against reversed battery polarity | Should | Incosistent JST wiring | Connect a reversed bench supply (current-limited); check the board still works after. | 
| REQ-E-009 | A resettable fuse shall protect the USB 5 V input: hold current 0.5 A (0.41 A at 40 °C), above the 350 mA worst-case normal draw, and trip current 1 A. | Must | "Idiot-proof". | Short the 3V3 pin; confirm the fuse trips and resets after the short is removed. |
| REQ-E-010 | The board shall be unharmed and shall operate normally when: no battery is connected; a battery is plugged while USB is connected/ USB is plugged while on battery. | Should | "idiot-proof". | Perform each case. | 
| REQ-E-011 | The board shall have an easily accessible reset and BOOT button shall be provided. | Must | Recoverable programming with no external tools. | Enter the bootloader and flash firmware.| 
| REQ-E-012 | The board shall have a charge-status LED (driven by the charger STAT pin, readable by firmware) and a user-controllable WS2812B-V6 RGB LED. The RGB LED is powered from the USB 5 V rail, so it is only usable while USB is connected, and draws nothing from the battery. | Should | Visible feedback; keeps battery drain at zero when running on the cell. | Visual check while charging; confirm the RGB LED is off and draws no battery current when USB is absent. | 
| REQ-E-013 | Dedicated ESD protection on the USB data lines is not fitted (descoped). The board relies on the MCU's USB pin rating (3 kV HBM) and the USB-C shield tied to GND. | May | No board space for an ESD array; accepted risk. | Inspect schematic. | 
| REQ-E-014 | The board shall provide a single 8 MHz crystal footprint with 15 pF load capacitors on the HSE pins (OSC_IN/OSC_OUT; the K8T6 has no LSE), and should run without the crystal fitted. | Should | Allows cheaper config & proves config for other projects. | Test USB and general operation with and without the crystal fitted. | 
| REQ-E-015 | Battery voltage should be readable by firmware through a 1 MOhm + 1 MOhm divider with a 0.1 uF capacitor on an ADC pin (PA1). | May | Battery level indication. The ADC needs a source impedance of 50 kOhm or less, which the capacitor provides for the 500 kOhm divider; the divider draws about 2 uA from a full cell. | Read the ADC value against a multimeter, with firmware calibration. | 
| REQ-E-016 | The development board must be based on the CH32V203K8T6 MCU (LQFP32). | Must | Proves system capabilities for future integrated project| Run sample code of exsisting project. | 
| REQ-E-017 | When battery-powered, the 3.3 V rail shall supply up to 500 mA, using a cell rated for at least 1C discharge. | Should | The fuse is not in the battery path; the limits are the LDO and the cell. | Load the 3V3 pin at 500 mA from a charged cell; check rail voltage and regulator temperature. |

## Mechanical / Form factor

| ID | Requirement | Priority | Rationale | Verification |
|----|-------------|----------|-----------|--------------|
| REQ-M-001 | The board outline shall be 17.8 x 49.0 mm. | Must | Longer than the XIAO format to fit the MCU, power path and USB-C. | Check the board outline in the layout. | 
| REQ-M-002 | 36 edge pads (18 per side) at 2.54 mm pitch with 0.6 in row spacing shall work with or without pin headers. | Must | Breadboard compatibility; castellated holes are supported by JLCPCB. | Check the layout; test-fit on a breadboard. | 
| REQ-M-003 | All parts shall be hand-solderable (gull-wing or leaded packages only), passives 0603 or larger. Listed exceptions only: USB-C connector. | Must | Keeps project at least plausible without a reflow oven | BOM package review. | 
| REQ-M-004 | Parts should be contained to the top side of the board. | Should | Allows easy breadboard use and soldering. | Test-fit on a breadboard. | 
| REQ-M-005 | Pin names shall be printed on the silkscreen, bottom side if space is short, using one consistent naming scheme. | Should | Easy use without a pinout chart. | Visual check of the silkscreen against the schematic. | 

## Interfaces / Pinout

| ID | Requirement | Priority | Rationale | Verification |
|----|-------------|----------|-----------|--------------|
| REQ-I-001 | The 36 edge pins shall carry the MCU's usable GPIOs plus 3V3, 5V and GND, with special functions (e.g. I2C) labelled by function. | Must | Full access to the MCU on a breadboard. | Compare the edge pads against the schematic. | 
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
- **Crystal and USB:** Double check whether the CH32V203 USB works without an external crystal (HSI is +-1.6 % to +-2.2 %; USB full speed asks for 0.25 %). If it needs one, REQ-E-014 must change. Also decide whether X1 is fitted by default (it is in the current BOM).
- **ESD / USB resistors:** Confirm in the reference manual or WCH's EVT schematic that the MCU has built-in series resistors on the USB data lines (not found in the datasheet text), before relying on this for REQ-E-013.
- **Reverse battery:** the power-path FET isolates the system from a reversed cell, but the charger and the battery divider sit on the raw battery net. A series P-FET at the battery connector would meet REQ-E-008.
- **Battery connector:** the BOM's J2 is now an S2B-PH-SM4-TB (SMD, 2.0 mm), which fixes the pitch, but REQ-I-004 describes plated through-holes. Confirm which one is intended and align the requirement.
- **LDO copper:** the TLV75733P only reaches its 100.8 C/W rating on TI's evaluation board; keep generous copper on its pins (350 mA is about 0.4-0.5 W).
- **Charge-current resistor:** R7 is now 10 kOhm; confirm against the MCP73831 datasheet (I = 1000 / R in kOhm) and the minimum output capacitor.
- **Divider capacitor:** confirm the capacitor on the PA1 divider is 0.1 uF (the BOM lists 4.7 uF for C10/C12).
- **BOM:** add manufacturer and vendor part numbers (WS2812B must be the V6 variant for 3.3 V logic); set X1 to 8 MHz.
- **More power later:** a fuse with about 1.1 A hold would allow a 500 mA load plus 250 mA charging, but needs a Schottky rated 1 A or more at D1 and a supply that can deliver it; costs size and money.
