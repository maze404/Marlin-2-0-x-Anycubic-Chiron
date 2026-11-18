# This is a fork of Knutwurst's work which adds BL-Touch, TMC2208 driver and different thermistor support!
Many many thanks to @Knutwurst for his work - my printer would not exist like this without his efforts!

## All changes done to my printer currently:
# Anycubic Chiron — Modified Configuration

## Hardware

### Hotend

- Volcano-style copper hotend / heater block
- Copper/titanium bimetal heatbreak
- 24 V 50 W heater cartridge
- NTC 100K 3950B thermistor
- Thermal compound applied to:
  - Heatbreak
  - Heater cartridge
  - Thermistor
- 0.8 mm steel nozzle


### Extruder

- Direct-drive extruder configuration
- STEPPERONLINE NEMA 17 pancake stepper motor
- 23 mm body length
- 17 N·cm holding torque
- 1.0 A rated phase current
- 1.8° step angle
- Bipolar, 4-wire
- 5 mm shaft


### Automatic Bed Leveling

- TEQStone 3D Touch V3.2 BLTouch-compatible probe
- Custom probe mount
- 18 mm probe mounting-hole spacing
- Original physical Z endstops retained
- Physical Z endstops used for Z homing
- 3DTouch used for bed mesh generation only
- Probe not used for Z homing

Current probe offsets:

    M851 X-45.49 Y-9.89 Z-3.00

Those offsets fit to my printer, but will probably have to be changed to fit to any other printer!


### Stepper Drivers

- TMC2208/TMC2209 driver configuration
- Standalone operation
- No UART control
- No sensorless homing

Known motor and driver configuration:

| Axis | Motor | Rated Current | Vref |
|------|-------|---------------|------|
| X | 42HD4027-01 (Stock Chiron Motor)| 1.5 A | ~1.29 V |
| Y | 42HD48-1804S (Stock Chiron Motor) | 1.8 A | ~1.55 V |
| Z1/Z2 | 42HD4027-01 (Stock Chiron Motor) | 1.5 A | ~1.29 V |
| Extruder | STEPPERONLINE NEMA 17 Pancake, 23 mm, 17 N·cm | 1.0 A | ~1.03 V |


## Firmware

Firmware is based on the Knutwurst Marlin fork for Anycubic printers.

Build environment:

    CHIRON_TMC_BLT


### Automatic Bed Leveling

- BLTouch / 3DTouch support
- Bilinear bed leveling
- 11 × 11 probing grid
- 121 mesh points
- Physical Z endstops used for homing
- Probe used exclusively for measuring the bed surface
- Leveling fade support
- `G26` mesh validation
- `M48` probe repeatability testing


### Enabled Firmware Features

- EEPROM storage
- Babystepping
- Linear Advance
- S-curve acceleration
- Hotend PID control
- Heated-bed PID control
- Native `G2` / `G3` arc support
- `M600` filament change
- Filament runout handling
- `G27` nozzle parking
- `M78` print statistics
- `M808` SD G-code loops
- Emergency parser
- Host-action commands
- Automatic temperature reporting


### Disabled / Unavailable Firmware Features

- Input shaping
- `G34` independent Z alignment
- `G35` assisted screw tramming
- Backlash compensation
- Power-loss recovery
- Cancel objects
- TMC UART control
- Sensorless homing
- Multiple BLTouch samples per mesh point
- Heaters/fans disabled during probing
- Stored G-code macros

## Bed Leveling

The original physical Z endstops establish machine Z=0 during homing.

The 3DTouch probe independently measures the bed surface and generates an
11 × 11 bilinear compensation mesh.

## Custom Anycubic Chiron build

The `CHIRON_TMC_BLT` target is configured for an Anycubic Chiron with the
original Trigorilla ATmega2560 board, standalone TMC2208 stepper drivers, and a
BLTouch. A `CHIRON_BLT` target without the TMC driver selection is also
available.

Changes from the upstream Knutwurst 1.6.0 configuration:

- Added the `CHIRON_BLT` and `CHIRON_TMC_BLT` PlatformIO environments.
- Enabled BLTouch support for Chiron builds and disabled the original Chiron
  servo-probe configuration and touchscreen leveling page when BLTouch is used.
- Set the nozzle-to-probe offset to `X-45.493 Y-9.894 Z-3.00`, however this should be adjusted depending where you mount your BL-Touch!
- Increased bilinear automatic bed leveling from 5 x 5 to 11 x 11, giving 121
  probe points when running `G29`.
- Selected hotend thermistor table 11 for Chiron builds since I am running a NTC 100K 3950B
- Set the Chiron hotend PID defaults to `P13.65 I0.99 D46.96`.

Build the customized firmware with:

```sh
platformio run -e CHIRON_TMC_BLT
```

The output is `.pio/build/CHIRON_TMC_BLT/firmware.hex`. The verified build uses
7,075 of 8,192 bytes of RAM (86.4%) and 150,594 of 253,952 bytes of flash
(59.3%).

### GitHub Releases

Publishing a GitHub Release builds every PlatformIO environment and attaches
one versioned HEX file per environment, such as
`CHIRON_TMC_BLT_v1.8.0-inofficial.hex`. Pushes to `master` also build every
environment and retain the HEX files as workflow artifacts. Use
`.github/release_template.md` for the release description.


# Knutwurst's original ReadMe Content

## Knutwurst's i3 MEGA M/S/P/X/Chiron/4MP2 Hybrid Firmware <br>(based on Marlin 2.1.x)

## Übersicht

Marlin is optimized to build with the **PlatformIO IDE** extension for **Visual Studio Code**. You can still build Marlin with **Arduino IDE**, and we hope to improve the Arduino build experience, but at this time PlatformIO is the better choice.

### Wenn dir gefällt, was ich mache, kannst du mir hier einen Kaffee spendieren*: [![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://paypal.me/oliverkoester)
<sub>*Es muss jetzt keine großzügige Spende sein. Ein paar Cent reichen um mir zu zeigen, wer überhaupt Interesse daran hat und wem die Weiterentwicklung wichtig ist. So bleibt die Motivation da und ich weiß einfach, dass ich nicht für die Tonne programmiere ;)<sub>


## Overview

- [Beginner's Guide](https://github.com/knutwurst/Marlin-2-0-x-Anycubic-i3-MEGA-S/wiki/Beginner's-Guide-(English))
- [Features](https://github.com/knutwurst/Marlin-2-0-x-Anycubic-i3-MEGA-S/wiki/Features-(English))
- [Frequently asked questions (FAQ)](https://github.com/knutwurst/Marlin-2-0-x-Anycubic-i3-MEGA-S/wiki/FAQ-(english))
- [Wiki/Tutotrials](https://github.com/knutwurst/Marlin-2-0-x-Anycubic-i3-MEGA-S/wiki)
- [Official Facebook-Group (german)](https://www.facebook.com/groups/knutwurst/)
- [Downloads](https://github.com/knutwurst/Marlin-2-0-x-Anycubic-i3-MEGA-S/releases)


### If you like what I do you can buy me a coffee*: [![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://paypal.me/oliverkoester)
<sub>*It doesn't have to be a generous donation. A few cents are enough to show me who is interested in further development. So the motivation stays and I just know that I am not programming for the bin ;)<sub>
