## Build Firmware

First you need to install **VSCode**: https://aur.archlinux.org/packages/visual-studio-code-bin/ (Arch only)  
Then install **platformio**: https://platformio.org/install/ide?install=vscode

Then to build from CLI:

```
export PATH=$PATH:~/.platformio/penv/bin
```

```
# Clone latest firmware source
git clone https://github.com/MarlinFirmware/Marlin.git Marlin-git

# Symlink existing configuration into build folder
ln -rfs Marlin/Configuration.h Marlin-git/Marlin/Configuration.h
ln -rfs Marlin/Configuration_adv.h Marlin-git/Marlin/Configuration_adv.h

# To select specific profile:
ln -rfs Marlin/Configuration-doctorseus.h Marlin-git/Marlin/Configuration.h
ln -rfs Marlin/Configuration_adv-doctorseus.h Marlin-git/Marlin/Configuration_adv.h

cd Marlin-git/

# IMPORTANT:
# We need to remove an unrequired dependency to avoid running into a compile issue:
# Add the following below the [env:at90usb1286_dfu] block in platformio.ini:
lib_ignore    = TMCStepper

# Start build with selected board env
platformio run -e at90usb1286_dfu
```

Firmware file __firmware.hex__ can be found in
```./.pio/build/at90usb1286_dfu/```

## Flash Firmware
```
sudo dfu-programmer at90usb1286 erase --debug 100
sudo dfu-programmer at90usb1286 flash ./.pio/build/at90usb1286_dfu/firmware.hex --debug 100

# Pre-build firmware found at https://github.com/Printrbot/printrboardmodernmarlin/tree/master/Simple_Metal
# sudo dfu-programmer at90usb1286 flash RevFv1.0_Printrbot_Simple_Metal_HB.hex --debug 100
```

## Configure Firmware
```
M502 ; EEPROM Factory Reset (version missmatch)
M500 ; Store in EEPROM

# PID autotune
M303 E-1 S60 C8 ; autotune BED
M303 E0 S210 C8 ; autotune EXTRUDER0

# Z-Probe Offset https://3dprinting.stackexchange.com/a/5858
M851 Z-1.6 ; Z-Probe Offset
M500 ; Store in EEPROM
M501 ; Load from EEPROM

G1 Z0;   // This will move the head to zero height;
M211 S0; // This will disable the end stops so that you 
         // will be able to proceed lower than Z=0
M211 S1;     // Enable the end stops again
```
