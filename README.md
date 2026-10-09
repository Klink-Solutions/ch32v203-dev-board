# CH32V203C8T6 Dev Board

### A XIAO-sized development board based on the WCH CH32V203C8T6 microcontroller.

## Description
 XIAO-compatible development board based on the WCH CH32V203C8T6 microcontroller to allow for low power custom applications.  

#### Features
  * User addressable multi colour led
  * Boot & reset buttons on chip
  * Integrated single cell lipo battery charging
    * with optional jst 2.0 connector
  * Low power
  * USBC for power,charging and programming
  * I2C
  * SPI
  * UART
  * TBD

## Version History
* 0.1
    * Initial design rules
* 0.2
    * Part sourcing to meet set requierments

## Road Map

* Design shematic
* Define board size (based on Xiao layout)
* Route PCB
* DRC
* Source & fabricate
* Arduino compatible firmwear
  
## Screenshots

## BOM

See [`bom/`](bom/)

## Goals

See [`docs/project-goals.md`](docs/project-goals.md).

## Design Requirements

See [`docs/design-requirements.md`](docs/design-requirements.md).

## Repository content

| Path | Contents |
|------|----------|
| [`docs/`](docs/) | Requirements, design notes, decision records, meeting notes |
| [`pcb/`](pcb/) | Schematics, layout, custom libraries, simulations, generated outputs |
| [`bom/`](bom/) | Bill of materials and archived revisions |
| [`datasheets/`](datasheets/) | Component datasheets|
| [`firmware/`](firmware/) | Firmware source and notes |
| [`manufacturing/`](manufacturing/) | Gerbers, assembly files |
| [`references/`](references/) | Reference designs, app notes, links |

## Help

Contact author 

## Author

Lucas Klemmer  
klinksolutions22@gmail.com

## License
MIT LIcence

If anyone ends up reproducing this device or making it even bettter I would be happy to see your improvements and add them to the oficial release.
