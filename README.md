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
- [Miuzei 4" Touch Screen display](https://www.amazon.com/Miuzei-Raspberry-Full-Angle-Heatsinks-Raspbian/dp/B07XBVF1C9)

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



## Raspberry Pi Configuration

1. Download Raspbian vanilla image from [here](https://www.raspberrypi.org/software/)
2. Image 64GB SD Card or larger with Rasbian image
3. Install drivers for Miuzei Display
    `git clone https://github.com/goodtft/LCD-show`
4. Install Klipper service
    `git clone https://github.com/KevinOConnor/klipper
    ./klipper/scripts/install-octopi.sh`
5. Install Moonraker service
    `git clone https://github.com/Arksine/moonraker.git`
    _note: if you have IPv6, you will need to add your IPv6 prefix to the trusted clients list, or just remove authenication entirely_
6. Install [Mainsail](https://docs.mainsail.app/setup/mainsail)
7. Install [KlipperScreen](https://github.com/jordanruthe/KlipperScreen)
Weirdness:
    - Have to disable lightdm in raspi-config
    - vext wheel generation failed during the initial setup script
    - had to modify `/etc/systemd/system/KlipperScreen.service` to change user to root
    
8. Install Webcam
- `sudo raspi-config` to enable webcam
- Follow [these directions](https://3dp.tumbleweedlabs.com/firmware/klipper-firmware/adding-webcam-support-to-mainsail)
    

