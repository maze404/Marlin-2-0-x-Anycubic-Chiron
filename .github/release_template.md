# Release Notes

## New Features

- [Summarize the user-visible changes in this release.]

## Known Issues

- [List known limitations, or write "None known".]

## How to Install

1. Download the HEX asset matching your printer model and hardware options.
   The asset filename includes the PlatformIO environment and firmware version,
   for example `CHIRON_TMC_BLT_v1.8.0-inofficial.hex`.
2. Flash the firmware using a compatible flashing tool for your printer.
3. Check the release notes for model-specific setup or calibration steps.

## Important: EEPROM Settings

Firmware defaults do not automatically replace settings already stored in the
printer's EEPROM. Back up your settings and bed mesh before loading defaults.
`M502` resets stored settings and may erase the mesh; probe again and save with
`M500` only after confirming that leveling completed successfully.

For the complete change history, see [CHANGELOG.md](../CHANGELOG.md).
