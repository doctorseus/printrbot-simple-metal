## Build Firmware

```
export PATH=$PATH:~/.platformio/penv/bin
```

```
# Clone latest firmware source
git clone https://github.com/MarlinFirmware/Marlin.git Marlin-Build

# Copy existing configuration into build folder
cp Marlin/Configuration.h ./Marlin-Build/Marlin/Configuration.h
cp Marlin/Configuration_adv.h ./Marlin-Build/Marlin/Configuration_adv.h

cd Marlin-Build/

# IMPORTANT: To avoid a compile issue with an lib we actually don't need run the following:
sed -e '/\sTMCStepper/ s/^/#/g' platformio.ini
# It will remove the dependcy for our build.

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

M851 Z-1.2 ; Z-Probe Offset
M500 ; Store in EEPROM
M501 ; Load from EEPROM
```
