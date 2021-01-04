# ender5plus-klipper
Ender 5 Plus with SKR Pro 1.1 and Klipper FW

Mike Cretella, 2021

## Configuration of Ender 5 Plus to Klipper FW 

Hardware Configuration:
- Ender 5 Plus 3D Printer
    - Power Supply: Meanwell LRS-350-24 
- Modular Direct Drive extruder from printermods.com
- BigTreeTech SKR Pro 1.2 Board
- FW: Klipper FW v0.9.1 (https://github.com/KevinOConnor/klipper)
- 4x TMC2208 UART based drivers
- Raspberry Pi 4 (4GB DRAM)
    - Octopi (Raspbian Based) Linux distro
- Miuzei 4" Touch Screen display

## Original Wiring Photos

Just in case you miss the sound if whining motors and poorly translated buggy firmware: 
 ![mainboard](https://github.com/cretells/ender5plus-klipper/blob/main/wiring-original/mainboard-hbfet-tft.jpeg?raw=true)
 See https://github.com/cretells/ender5plus-klipper/tree/main/wiring-original

## Wiring Diagram


## Firmware Configuration
- Clone from Klipper Github repo (from RPi):
    `git clone https://github.com/KevinOConnor/klipper`

- Make and flash firmware according to the following instructions from Klipper3d. 
    `firmware.bin` is the one i've created  based on Klipper3D v0.9.0
```
# The firmware should be compiled for the
# STM32F407 with a "32KiB bootloader".

# The "make flash" command does not work on the SKR PRO. Instead,
# after running "make", copy the generated "out/klipper.bin" file to a
# file named "firmware.bin" on an SD card and then restart the SKR PRO
# with that SD card.
```
## Printer Configuration 
- `printer.cfg` is a fork of 

https://github.com/KevinOConnor/klipper/blob/master/config/generic-bigtreetech-skr-pro.cfg
with my modifications specifically for the Ender 5 Plus

#### Pinouts
Fans: 
- I connected the part cooling fan to `PC8`
- Extruder0 fan was connected to `PE5`
- Chassis fan connected to `PE6`

### Calibration

By now, you should have Klipper running on your 3DPrinter and `G28` will properly home the device (all motors have been confirmed functional)

### Hotend / Bed PID
`TBD`

### Extruder Calibration
Extruder calibration is needed to ensure correct amount of filament is extruded during printing. It may be dependent on extruder configuration and gearing. Calibration can be done as follows:

https://www.klipper3d.org/Rotation_Distance.html#calibrating-rotation_distance-on-extruders

My Result:
` rotation_distance: 33.5`

### Retraction Calibration

TeachingTech has a great resource for retraction calibration.

https://teachingtechyt.github.io/calibration.html#esteps

My Result: `1.2mm`  (this is actually tracked in slicer settings)

### Input Shaping

*This needs to be run on each printer, since it is directly dependent on resonance of the machine during high accelration operation*

My Result:

X- 2/((2.24+2.2+2.31)/3)*180 = 160Hz

Y- 4/((3.93+3.98+3.75)/3)*180 = 185.24Hz

```
[input_shaper]
shaper_freq_x: 160  # frequency for the X mark of the test model
shaper_freq_y: 185.24  # frequency for the Y mark of the test model
```
### Pressure Advance

*Extruder calibration should happen first before this*

My Result: `pressure_advance: 0.1261`



