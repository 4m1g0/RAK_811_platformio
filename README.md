# RAK_811_platformio
Repository with resources to compile and upload firmware with platformio to the RAK-811 based boards (including WisTrio Rak 7205)

## Project configuration
Thanks to the [ST STM32](https://docs.platformio.org/en/latest/platforms/ststm32.html) platform development for platformio, finally it is possible to build a firmware for RAK811 completely from platformio. 

Using the Platformio project wizard these options have to be selected:
 * Name: Name of your project
 * Board: RAK811 LoRa Tracker
 * Framework: Arduino

Once the project is created, we found a standard Arduino structure with a main file and a setup and loop function.

## Compiling the firmware
In order to compile the firmware, it is enough using the build task defined by platformio, and the binary file is generated inside the project folder in the following path: `.pio/build/rak811_tracker/firmware.bin`

![Boot pin](/doc/images/build_task.png "Boot pin")

## Uploading to WisTrio RAK 7205
The upload process has 4 steps.
 * Entering bootloader mode
 * Unprotecting flash
 * Upload firmware
 * Protecting flash

These steps will be a bit different for every board, here we are using the WisTrio 7205 which has a boot pin that has to be set to low in order to enter the bootloader mode.

![Boot pin](/doc/images/boot_pin.jpg "Boot pin")

Once the pin is set to `LOW` using the jumper, the board can be connected via USB to unprotect the flash using the following command changing the port according to the port to which your board is connected.

```
~/.platformio/packages/tool-stm32duino/stm32flash/stm32flash /dev/ttyUSB0 -k
```

The next step is uploading the firmware, to do so it is necessary to reset the board using the reset button and use the following command to upload the firmware.

```
~/.platformio/packages/tool-stm32duino/stm32flash/stm32flash /dev/ttyUSB0 -e 0 -w .pio/build/rak811_tracker/firmware.bin
```

Make sure to change the firmware path and the port according to your configuration.

The last step is to protect again the flash memory, if this step is skipped it will not work.

```
~/.platformio/packages/tool-stm32duino/stm32flash/stm32flash /dev/ttyUSB0 -j
```

