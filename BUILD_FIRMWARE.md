
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

# Start build with selected board env
platformio run -e at90usb1286_dfu
```
