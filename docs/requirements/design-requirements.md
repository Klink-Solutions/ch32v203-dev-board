# Design Requirements

> **Status: DRAFT rev 0.1.** Developed from the design discussion on 2026-10-07.
> Items marked **[?]** are undecided. Battery/power items are pending a review of the
> existing Bike Light battery circuit (see REQ-E-016 and Open questions).
> Tracked in the Issues tab: "Write design requirements" (#2).

**How to use:** every requirement has a stable ID (never reuse or renumber one).
Priority is **Must / Should / May**: *Must* = the board has failed without it,
*Should* = wanted, can be cut if layout or cost demand it, *May* = nice to have.
Verification says how each requirement will be proven (see `test/plans/`).
The XIAO outline is small, so Should/May items are the first to cut if they do not fit.

## Revision history

| Rev | Date | Author | Summary |
|-----|------|--------|---------|
| 0.1 | 2026-10-07 | | First draft from design discussion |

## Electrical

| ID | Requirement | Priority | Rationale | Verification | Status |
|----|-------------|----------|-----------|--------------|--------|
| REQ-E-001 | The board shall be powered from USB-C 5 V with 5.1 kOhm pull-downs on both CC pins so it works with C-to-C cables and chargers. | Must | Without CC resistors many USB-C supplies output nothing. | Power from an A-to-C and a C-to-C source; measure rail. | Draft |
| REQ-E-002 | When USB-powered, the 3.3 V rail shall supply 500 mA total (MCU plus external loads) under normal conditions, within the MCU's supply range. | Must | Useful dev board; external sensors and modules. | Load the 3V3 pin with a 500 mA load for 10 min; log voltage and regulator temperature. | Draft |
| REQ-E-003 | When battery-powered, the 3.3 V rail shall stay regulated down to a documented minimum cell voltage **[? V]**. | Should | Predictable behavior as the cell discharges. | Discharge test with a bench supply emulating the cell. | Draft |
| REQ-E-004 | The 5V pin shall carry USB 5 V (after input protection) only while USB is connected, and shall not be driven on battery power. | Must | No boost converter fits; avoids false expectations. | Measure 5V pin with and without USB. | Draft |
| REQ-E-005 | The board shall charge a single-cell Li-ion/LiPo from USB using a constant-current / constant-voltage profile, terminate automatically when full, and recharge only after the cell voltage drops. It shall not trickle-charge continuously when full. | Should | Safe, standard Li-ion charging; the cell can stay connected. | Charge a protected cell; log current and voltage through termination. | Draft |
| REQ-E-006 | Default charge current shall be 250 mA (0.5C for a 500 mAh cell), set by a single resistor so it can be changed for other cell sizes up to **[? max mA]**. | Should | Gentle on a typical 500 mAh cell. | Measure charge current during the constant-current phase. | Draft |
| REQ-E-007 | When USB is present the system shall run from USB and the battery shall not supply the load; when USB is absent the system shall run from the battery (power-path behavior). | Should | Battery does not drain while on USB, and charging is independent of load. | Measure battery current with USB connected and load active. | Draft |
| REQ-E-008 | Reversed battery polarity shall cause no damage to the board. | Should | JST connectors are wired inconsistently between vendors. | Connect a reversed bench supply (current-limited); check the board still works after. | Draft |
| REQ-E-009 | Only protected single-cell Li-ion/LiPo cells shall be supported. The board shall not provide its own cell under-voltage cutoff. This shall be stated on the silkscreen and in the README. | Must | Keeps the circuit small and simple; protection lives in the cell. | Inspect silkscreen and README. | Draft |
| REQ-E-010 | A resettable fuse shall protect the USB 5 V input. Hold current shall be above the worst-case normal draw (**[? mA]**) and trip current below **[? mA]**. | Should | "Idiot-proof": a shorted 3V3 pin trips it and it recovers. | Short the 3V3 pin; confirm the fuse trips, then recovers after removing the short. | Draft |
| REQ-E-011 | The board shall be unharmed and shall operate normally when: no battery is connected; a battery is hot-plugged while USB is connected; USB is hot-plugged while on battery. | Should | Defines "idiot-proof" as testable cases. | Perform each case; check operation afterwards. | Draft |
| REQ-E-012 | A reset button and a BOOT button shall be provided. Holding BOOT while plugging in USB (or resetting) shall enter the USB bootloader. | Should | Recoverable programming with no external tools. | Enter the bootloader and flash firmware. Confirm the exact sequence in the datasheet. | Draft |
| REQ-E-013 | A user-controllable status LED and a charge-status LED shall be provided, using chip pins that are not on the XIAO edge pins. | Should | Visible feedback; does not use up header pins. | Visual check while running test firmware and while charging. | Draft |
| REQ-E-014 | ESD protection should be provided on the USB data lines. | May | USB-C port is the main external interface. | Inspect schematic. | Draft |
| REQ-E-015 | The board shall not require an external crystal. Footprint and load-capacitor pads for a crystal on the HSE pins shall be provided, left unpopulated by default. | Should | Matches the Bike Light design; lets a crystal be validated later without a respin. Depends on whether USB works without one (see Open questions). | Test USB and general operation with and without the crystal fitted. | Draft |
| REQ-E-016 | The battery power subsystem shall be based on the validated circuit from the Bike Light board. Any deviation shall be recorded in a decision record. | Should | Proven design reduces risk and shows engineering reuse. The reused circuit shall be checked against REQ-E-005 to REQ-E-011. | Schematic review against the Bike Light source; gap list. | Pending schematic |
| REQ-E-017 | Battery voltage should be readable by firmware through a high-value resistor divider on a chip pin that is not on the XIAO edge pins. | May | Battery level indication. | Read the ADC value against a multimeter at several battery voltages. | Draft |

## Mechanical / Form factor

| ID | Requirement | Priority | Rationale | Verification | Status |
|----|-------------|----------|-----------|--------------|--------|
| REQ-M-001 | The board outline shall match the XIAO footprint, 21 x 17.8 mm. | Must | XIAO-sized board. Confirm exact dimensions in issue #3. | Measure with calipers. | Draft |
| REQ-M-002 | 14 edge pads at 2.54 mm pitch shall match the XIAO pin positions and work with or without pin headers. | Must | XIAO compatibility. Castellated holes are supported by JLCPCB (min hole 0.5 mm); confirm price at quote. | Place the board on a XIAO footprint or socket. | Draft |
| REQ-M-003 | All parts shall be hand-solderable: gull-wing or leaded packages only (no QFN/BGA), passives 0603 or larger. Listed exceptions only: USB-C connector, optional crystal. | Must | Builder solders by hand with an iron and hot air. | BOM package review. | Draft |
| REQ-M-004 | Parts on the bottom side shall be low enough that the board sits on a breadboard **[? max height mm]**, except the optional JST connector. | Should | Breadboard use. | Measure; test-fit on a breadboard. | Draft |
| REQ-M-005 | Pin names shall be printed on the silkscreen, bottom side if space is short. | Should | Easy use without a pinout chart. | Visual check. | Draft |

## Interfaces / Pinout

| ID | Requirement | Priority | Rationale | Verification | Status |
|----|-------------|----------|-----------|--------------|--------|
| REQ-I-001 | The 14 edge pins shall carry D0-D10, 3V3, 5V and GND in the XIAO order. | Must | Drop-in XIAO pinout, including the 3V3 and 5V rails. | Compare with the official XIAO pinout (issue #3). | Draft |
| REQ-I-002 | Where the chip allows it, default functions shall follow the XIAO convention (e.g. I2C on D4/D5, UART on D6/D7, SPI on D8-D10, analog on D0-D3). | Should | Existing XIAO code and shields line up. | Test each peripheral on its pins; see issue #4 for the CH32V203 pin mapping. | Draft |
| REQ-I-003 | USB-C shall provide both USB device function and programming (bootloader). | Must | Single cable for power, data and programming. | Flash and enumerate over USB. | Draft |
| REQ-I-004 | Every chip pin not on the edge pins, plus the debug pins, reset and BOOT, shall be available on bottom-side test pads. | Should | Lets the builder fully test the chip. | Probe each pad. | Draft |
| REQ-I-005 | Two plated through-holes shall allow an optional JST battery connector to be fitted on the back, with polarity printed on the silkscreen. Connector type and pitch **[? e.g. JST PH 2.0 mm]**. | May | Easier than soldering to pads; omitted when used on a breadboard. | Fit the connector; check polarity against a meter. | Draft |

## Firmware / Software

| ID | Requirement | Priority | Rationale | Verification | Status |
|----|-------------|----------|-----------|--------------|--------|
| REQ-F-001 | A blink program shall be flashable over USB with no external programmer. | Must | Core "usable dev board" promise. | Flash blink on a fresh board. | Draft |
| REQ-F-002 | Test firmware shall exercise every exposed peripheral (I2C, SPI, UART, ADC, USB) and be kept in `firmware/`. | Should | Supports full validation of the chip and board. | Run the test firmware; save results in `test/reports/`. | Draft |

## Manufacturing / Cost

| ID | Requirement | Priority | Rationale | Verification | Status |
|----|-------------|----------|-----------|--------------|--------|
| REQ-C-001 | The PCB shall be a JLCPCB standard 4-layer board and obey its multilayer limits (see `docs/design/jlcpcb-4-layer-limits.md`). | Must | Fixed fab and layer count to control cost. | Run the DRC with JLCPCB rules; compare with the quote. | Draft |
| REQ-C-002 | Five complete boards (PCB, parts, shipping, no assembly service) shall cost no more than USD 60. | Must | Project budget. | Keep quotes and order confirmations in `manufacturing/quotes/`. | Draft |
| REQ-C-003 | All parts shall be in stock at the chosen supplier when the order is placed. | Must | Avoids a stalled build. | Check stock at order time; save a snapshot. | Draft |

## Documentation / Record-keeping

| ID | Requirement | Priority | Rationale | Verification | Status |
|----|-------------|----------|-----------|--------------|--------|
| REQ-D-001 | Each board revision shall have its schematic, layout, BOM and Gerbers committed, plus journal entries and decision records. | Must | Hack Club record-keeping requirement. | Review the repository at each milestone. | Draft |

## Open questions

- **Bike Light battery schematic:** the reuse baseline for REQ-E-016. Review it against REQ-E-005 to REQ-E-011 once found; list the gaps.
- **Regulator topology:** at 500 mA, a linear regulator from 5 V dissipates about 0.85 W ((5 - 3.3) V x 0.5 A), which is hard to cool in a tiny package. Options: a larger linear regulator package with copper area (4 layers help), or a small buck converter (costs board area). Decide in a decision record.
- **USB current budget:** 500 mA on 3V3 plus 250 mA charging is about 750 mA from a 5 V source, above the 500 mA a basic USB port guarantees. Options: reduce charge current while the load is high, or require a capable supply. This also sets the fuse hold current (REQ-E-010).
- **Crystal and USB:** check in issue #4 whether the CH32V203 USB works without an external crystal. If it needs one, REQ-E-015 must change.
- **Castellation cost:** check the extra cost, if any, when quoting at JLCPCB (REQ-M-002).
- **Battery connector:** JST type and pitch for REQ-I-005.
- **Minimum battery voltage:** value for REQ-E-003.
