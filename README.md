# Creality Ender 3 V4.2.7 RET6 (512K) Firmware Build Guide

1. This setup is for the Creality Ender 3 Silent Motherboard, High Performance 32 Bit 3D Printer Noiseless Mother Board V4.2.7 with TMC2225 Driver Marlin for Ender 3 RET6 version 512K flash.
   Amazon link: https://www.amazon.com/dp/B0C77RBBP5

2. Install the replacement hot end assembly: POLISI3D # Ceramic Heating Core CHC Hotend 24V Copper Heater Block Titanium Heatbreak Improved Nozzle Compatible with Voron 2.4 Ender 3 V2 Pro CR10 Ender 5 Pro/Plus Ender 6 3D Printer.
   Amazon link: https://www.amazon.com/dp/B0C5JFKF8H

3. Install the Creality CR Touch Auto Leveling Kit, 3D Printer Bed Auto Leveling Sensor Kit for Ender 3/ Ender 3 Pro/Ender 3 V2/ Ender 3 Max/Ender 5/Ender 5 Pro and CR 10 with 32 Bit V4.2.2/V4.2.7 Mainboard
   Amazon link: https://www.amazon.com/Creality-Leveling-3D-Printer-Mainboard/dp/B09DVYZSYJ

4. Build the firmware bin with the included changes in these files:
   - Marlin/_Bootscreen.h
   - Marlin/_Statusscreen.h
   - Marlin/Configuration_adv.h
   - Marlin/Configuration.h

   Build from this repository's parent folder:

   ```bash
   ./build_firmware_bin.sh
   ```

   The script builds the STM32F103RE_creality environment and prints the full path to the generated firmware .bin file.

Use the following shell script to build the firmware.bin file

```bash
#!/usr/bin/env bash
set -euo pipefail

REPO_DIR="Marlin-Creality-Ender3-V427"

PIO="$(find "$HOME/.platformio" -type f -name pio 2>/dev/null | head -n 1 || true)"
if [ -z "$PIO" ] || [ ! -x "$PIO" ]; then
   echo "Error: pio not found under $HOME/.platformio" >&2
   exit 1
fi

cd "$REPO_DIR"

"$PIO" run -e STM32F103RE_creality

BIN_PATH="$(find .pio/build/STM32F103RE_creality -type f -name '*.bin' 2>/dev/null | sort | tail -n 1 || true)"
if [ -z "$BIN_PATH" ]; then
   echo "Error: no firmware .bin found in .pio/build/STM32F103RE_creality" >&2
   exit 1
fi

printf '%s/%s\n' "$PWD" "$BIN_PATH"
```

5. Copy the firmware.bin file to a micro SD card.

6. Insert the micro SD card into the Ender 3 micro SD card slot and power on the Ender 3.

7. The firmware.bin file should flash itself into the EEPROM memory of the board.

8. Run PID Autotuning.

9. Run Y offset.

10. Level the print bed.
