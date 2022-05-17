# Ender5p_Archim2_Klipper
Documentation on using an Archim 2 board and Klipper to drive an Ender 5 Plus

# Mounting And Parts Required
## Enclosure Modification

## Mounting Plate
Mounting Plate Stl File Here.
Use Whatever means nesessary to cut holes in the side of the metal box.  I drilled pilot holes and used a metal mibbler to cut away as needed.  It's not pretty, but it works!

## Parts List
* Printed mounting plate
* Archim 2 Board with accessory wiring and sparts https://ultimachine.com/products/archim2
* Raspberry P 4 (or a 3 shuld work just fine too)
* RepRapDiscount 12860 Display https://www.amazon.com/gp/product/B01FH8KTZU/
* Printed Display Housing
* Dupont Connectors and Crimp Tool https://www.amazon.com/gp/product/B07ZK5F8HP/
* Wire Ferrules https://www.amazon.com/gp/product/B073TZ5BBG/
* Short USB Jumper https://www.amazon.com/gp/product/B000E5BKQE/
* Little plastic screws to mount the pi and archim boards to the mounting plate.  https://www.amazon.com/Thread-Rolling-Plastic-Finish-Degree/dp/B00GDYNHL6
* Screws and clips to attache the display frame to the printer tubing.

# Display
For whatever reason, you have to reverse the black plastic clips on the pins for the display because they are reversed between the Archim and the Display.  You just need to flip the display connectors around 180 degrees, so pull the clips off, spin them, and the cables will socket in the correct way.

# Wiring
The most tedius part of this conversion was crimping new ends onto many of the cables to work with the archim board.  The crealty board uses JXT/JST connectors for most stuff, and the Archim (as well as the Ultimachine Rambo boards) use _ connectors, whech are compatible with standard Dupont Pin connectors.

## Power Wiring
All three sets of power connectors should be connected to the power supply, but a large current connection isn't needed for the heated bed since we have a separate transistor switching power to the bed.  
![Power Connector](./images/power_tripple_jumper.png)  
  
## Stepper Wires
You can literally just cut off the existing plugs from the stepper wires and put on the new connectors keeping the wires in the same order.  If the motor moves the wrong direction, just put an '!' in front of the direction pin in the printer.cfg for that stepper.  
  
The only important wiring orientation is for the z steppers.  I'm not clear if one of them needs to be reversed or not.  If the z steppers don't turn the same direction, take the pins out of one of the connectors and reverse the order in the connector.  Then, if they, together, move the wrong direction, like up is down, then put a '!' in front of the direction pin in the printer.cfg.   
  
## Fans
Polarity is important for the fans.  I wired one to the fan header, and the other to one of the power terminals on the edge of the board.

## Endstops

## Thermistors

## BLTouch Sensor

# Klipper Config
See the attached [printer.cfg](./printer.cfg).  You can edit the file via the web interface.  Notable features in this:
* Pins per the above diagrams
* Bltouch
* Display working
* Steppers set to stealthchop, except for Y for more torque on that axis.
* ... few macros that I'm still figuring out, and more!
  
## Flashing the board with klipper
The USB id changes a couple times during the process of flashing the firmware, or updating it.  Just check /dev/serial/by-id for any devices detected and update the commands as needed to point to your device.  
  
## Abbriged command history from my system
```
ls /dev/serial/by-id/
cd ~/klipper
make menuconfig
make
sudo service klipper stop
make flash FLASH_DEVICE=/dev/serial/by-id/usb-UltiMachine_Archim-if00
...
make flash FLASH_DEVICE=/dev/serial/by-id/usb-03eb_6124-if00
sudo service klipper start
```
If you update klipper, you'll have to re-flash the board, so you may need to console back in again to do another make flash.  
  
## Moonraker warnings once printer is connected
There were some warnings about where spicifis configs were in the moonraker.conf file.  You can edit the file via the web interface and move things around to mitigate the warnings and reboot for your changes to take effect.


# Reference Links
* Archim Schematic and Pinouts
** https://reprap.org/wiki/Archim2
* How to wire the BLTouch with Archim 2
** https://github.com/radensb/BLTouch-Installation-Guide-Archim2
