OpenPi getting started
======================

OpenPi by Ciseco Ltd is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.

This document is a **work in progress**, please feel free to offer updates and comments via github’s issue and pull request system

**Some sections still have TODO place holders**

What’s on the board
===================
* SRF with on board chip antenna, the Radio can be accessed via /dev/ttyAMA0 (/dev/ttyS1) at 9600 baud
* HM-10 Bluetooth Low energy module on /dev/ttyS0 (see BLE module setup)
* RTC provided by TI BQ3200
* 2K EEPROM (HAT I2C bus)
* On Board temperature sensor (TMP100NA)

External Connections
--------------------
* Button
* HDMI
* IR Receiver
* USB Power
* Power LED
* ACK LED

Internal Connections
--------------------
* Compute Module socket
* Programming USB port
* Two USB Sockets
* SRF Programming header (JP3, WT use  only)
* External antenna U.fl footprint
* RTC battery (CR1220)
* GPIO Battery breakout (headers will shortly be available from http://wirelessthings.net)


Schematic and eagle files
=========================
These can be found here

https://github.com/WirelessThings/OpenPi

Native files are in Cadsoft Eagle v7 format. PDFs of the Schematic are also included

Gerbers for the board will be added soon

OS Image
========

For OpenPi we have built a custom Raspbian Wheezy image.
A copy can be downloaded from here
http://files.wirelessthings.net/OpenPi/2015-06-23-raspbian-wheezy-openpi.img.zip

For details on how to flash a Compute module please see the offical Raspberry Pi docs here https://www.raspberrypi.org/documentation/hardware/computemodule/cm-emmc-flashing.md

This is the recommended image for use with OpenPI. Its based of the 2015-05-05 Wheezy image as released by the Raspberry Pi Foundation with the following changes:

* dt-bin
* openpi-overlay
* enable i2c bus
* enable hardware RTC (ds1307)
* enable IR Receiver (lirc driver)
* Disable console and tty on /dev/ttyAMA0
* Install apt-get packages
	- screen
	- minicom
	- putty
	- vim
	- python-setuptools
	- i2c-tools
	- lirc
	- arduino
	- python-requests
* Remove apt-get packages
	- fake-hwclock
* Pip install python packages
    - arrow
    - xively-python
    - python-twitter
* Symlink /dev/ttyAMAO to /dev/ttyS1
* WIK_Files to the desktop (RasWIK python tools)
* LLAP_Files to desktop (Early version of LaunchPad, do not use)

If you are interested in how it is built see the openpi branch of this git repo

https://github.com/WirelessThings/spindle/

Customizations are in::

    wheezy-stage2
    wheezy-stage5-openpi

Official Compute Docs
=====================
The compute module used at the heart of OpenPi is a little different than working with a normal PI, official docs can be found here

https://www.raspberrypi.org/documentation/hardware/computemodule/README.md


Different reward levels
=======================
£29
---
The £29 pound reward level only includes the bare OpenPi PCB.
To get up and running you will need to buy a Raspberry Pi Compute module and flash it with an operating system image.
You will also need a suitable power supply (Micro USB 5v 2A), keyboard, HMDI screen and usb network adapter (either usb to LAN or wifi)

We recommend using our pre configured image OS image that can be downloaded from the links above.
For details on how to flash a Compute module please see the offical Raspberry Pi docs here https://www.raspberrypi.org/documentation/hardware/computemodule/cm-emmc-flashing.md


£55
---
If you have the £55 reward you will need a suitable power supply (Micro USB 5v 2A), keyboard, HMDI screen.

£69 and above
-------------
If you have this level all you will need to get started is a HMDI screen

Initial setup and turning on
============================
The following assumes you have a WirelessThings supplied compute module that is pre flashed with our recommended image or that you have flashed your own module with our image.

With all the items needed, as described above in the the “Different reward levels” section you can get started. First if you have the supplied wireless keyboard you need to find the USB dongle store inside the keyboard battery compartment and place it in the spare internal usb socket.
Turn on the keyboard with the little switch on the back edge.

Plug in your screen with a hdmi cable

Plug in the USB power supply

At this point the OpenPi should start to boot, the Power LED (Green and closest to the USB power socket) should be on solid and the Activity LED (red) should be flashing

The screen should come to life and you will see the traditional Raspbian boot screen scroll by.

On first boot the OpenPi will go into the **raspi-config** tool, here we recommend that you chose option 1) **Expand filesystem**, for security change the user password and for ease of identification change the Hostname (Advance Options, Hostname)

Once finished the OpenPi should reboot to expand the filesystem and then present you with a login prompt

The default login details are::

    Username = pi
    Password = raspberry

Now you can configure your OpenPi to connect to your wifi network via the GUI or by config file

To use the GUI, start the x windows system::

    $ startx

Now you can use the Icon in the top right of the task bar to setup your wifi network

If you wish to use the configuration file, edit the following file with your prefered text editor
*/etc/wpa_suplicant/wpa_supplicant.conf*
::

    $ sudo vi /etc/wpa_supplicant/wpa_supplicant.conf

or::

    $ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

Add the following section to the file::

    network={
        ssid=”yourSSIDhere”
        psk=”yourPSKhere”
    }

Note the lack of spaces by the ‘=’ is important. Save the file and reboot.

**That’s it you are ready to go.**

Getting started with the Wireless Ambient Temperature Sensor SB-CA-White
========================================================================

Download the WirelessThings LaunchPad software to the openPI from  https://www.wirelessthings.net/launchpad

Source for the LaunchPad can be found here https://github.com/WirelessThings/WirelessThings-LaunchPad

Unzip into a folder and follow the “WirelessThings LaunchPad User Guide” in the Documentation folder. This will guide you through setting up your temperature sensor.

Once set up you can either leave the MessageBridge running and try out the examples in the Examples folder, or you can stop the message bridge and communicate directly with the SRF on /dev/ttyAMA0.


IR Receive setup
================
**..TODO..**

Link to Pi LIRC docs, it is on /dev/lirc0 as configured by the DT overlay line …. to /boot/config.txt


BLE module setup
================
First update the Raspbian image using *apt-get* to get the new 4.0 kernel. NOTE: You must have expanded the file system using *raspi-config* first::

	$ sudo apt-get update
	$ sudo apt-get upgrade

add the following line to the end of the */boot/config.txt* file::

   dtoverlay=uart1,txd1_pin=40,rxd1_pin=41

Save and reboot

The serial port */dev/ttyS0* should now be avalible

Links to HM-10 docs
**..TODO..**

LightBlue for mac/iOS testing examples
**..TODO..**

Adding an external Antenna
==========================
**..TODO..**

parts can be brought on shop <>

Soldering a u.fl instruction

Drilling case hole size needs to be Xmm

No need to disconnect internal but can be done by removing part L3


GPIO usage
==========
The GPIO pins are exposed via pads on the bottom of the openPi board. A suitable header will shortly be available to buy on the shop.

The following pins are available on the back header

+-----+--------+------------+
| Pin | GPIO   | Function   |
+=====+========+============+
| 1   | 3V3    | 3V3 Supply |
+-----+--------+------------+
| 2   | GPIO2  | IC2 SDA1   |
+-----+--------+------------+
| 3   | GPIO3  | I2C SCL1   |
+-----+--------+------------+
| 4   | GPIO4  |            |
+-----+--------+------------+
| 5   | GPIO5  |            |
+-----+--------+------------+
| 6   | GPIO6  |            |
+-----+--------+------------+
| 7   | GPIO7  | SPI0 CE1   |
+-----+--------+------------+
| 8   | GPIO8  | SPIO CE0   |
+-----+--------+------------+
| 9   | GPIO9  | SPIO MSIO  |
+-----+--------+------------+
| 10  | GND    | Ground     |
+-----+--------+------------+
| 11  | GPIO10 | SPI0 MISO  |
+-----+--------+------------+
| 12  | GPIO11 | SPIO SCK   |
+-----+--------+------------+
| 13  | GPIO12 |            |
+-----+--------+------------+
| 14  | GPIO13 |            |
+-----+--------+------------+
| 15  | GPIO18 |            |
+-----+--------+------------+
| 16  | GPIO19 |            |
+-----+--------+------------+
| 17  | GPIO20 |            |
+-----+--------+------------+
| 18  | GPIO21 |            |
+-----+--------+------------+
| 19  | GPIO22 |            |
+-----+--------+------------+
| 20  | Ground | Ground     |
+-----+--------+------------+

The following pins are used by device internally on the OpenPi

+------+----------------------------------------------------+-----------------------------------+
| GPIO | Function                                           | Device                            |
+======+====================================================+===================================+
| 0    | I2C0 SDA, used for HAT eeprom                      | 2K EEPROM                         |
+------+----------------------------------------------------+-----------------------------------+
| 1    | I2C0 SCL, uset for HAT eeprom                      | 2k EEPROM                         |
+------+----------------------------------------------------+-----------------------------------+
| 2    | I2C1 SDA                                           | GPIO Header, RTC, Temp Sensor     |
+------+----------------------------------------------------+-----------------------------------+
| 3    | I2C1 SCL                                           | GPIO Header, RTC, Temp Sensor     |
+------+----------------------------------------------------+-----------------------------------+
| 4    |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 5    |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 6    |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 7    |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 8    |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 9    |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 10   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 11   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 12   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 13   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 14   | UART0 TX                                           | SRF                               |
+------+----------------------------------------------------+-----------------------------------+
| 15   | UART0 RX                                           | SRF                               |
+------+----------------------------------------------------+-----------------------------------+
| 16   | SRF AT Command Pin (Not yet available in firmware) | SRF                               |
+------+----------------------------------------------------+-----------------------------------+
| 17   | SRF DTR (use for OTAMP reset)                      | SRF                               |
+------+----------------------------------------------------+-----------------------------------+
| 18   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 19   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 20   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 21   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 22   |                                                    | GPIO Header                       |
+------+----------------------------------------------------+-----------------------------------+
| 33   | IR TX                                              | NOT CONNECTD, RESERVED FOR DRIVER |
+------+----------------------------------------------------+-----------------------------------+
| 34   | BUTTON                                             | Push Button                       |
+------+----------------------------------------------------+-----------------------------------+
| 35   | IR RX                                              | IR Reciever                       |
+------+----------------------------------------------------+-----------------------------------+
| 36   | SRF RESET                                          | SRF                               |
+------+----------------------------------------------------+-----------------------------------+
| 37   | HM10 RESET                                         | HM-10 BLE                         |
+------+----------------------------------------------------+-----------------------------------+
| 38   | HM10 LED                                           | HM-10 BLE                         |
+------+----------------------------------------------------+-----------------------------------+
| 39   | HM10 KEY                                           | HM-10 BLE                         |
+------+----------------------------------------------------+-----------------------------------+
| 40   | UART1 TX                                           | HM-10 BLE                         |
+------+----------------------------------------------------+-----------------------------------+
| 41   | UART1 RX                                           | HM-10 BLE                         |
+------+----------------------------------------------------+-----------------------------------+
| 42   | UART2 RTS                                          | HM-10 BLE                         |
+------+----------------------------------------------------+-----------------------------------+
| 43   | UART1 CTS                                          | HM-10 BLE                         |
+------+----------------------------------------------------+-----------------------------------+

OpenPi DeviceTree files and settings
=====================================
**..TODO..**

To correctly configure the GPIO pins on a pi we use DT files
below is an explanation of ..

::

 	dtoverlay=openpi
	dtparam=i2c1=on
	dtparam=i2c_arm
	dtoverlay=i2c-rtc,ds1307
	dtoverlay=lirc-rpi,gpio_in_pin=35,gpio_out_pin=33

dt-blob.dts
-----------
This file is used by the videocore (GPU) of the Pi to setup the default pin states at boot before handing over to the ARM core (Linux)

Customisation for OpenPi are **..TODO..**

Source file can be found here

https://github.com/WirelessThings/OpenPi/blob/master/DeviceTree/openpi-dt-blob.dts

Use the following command to compile and install the dts::

$ sudo dtc -I DTS -O DTB -o /boot/dt-blob.bin ./openpi-dt-blob.dts

openpi-overlay.dts
------------------
This file is used by the linux to setup the gpio pins and drivers for OpenPi’s peripherals

Customisation for OpenPi are **..TODO..**

Source file can be found here
https://github.com/WirelessThings/OpenPi/blob/master/DeviceTree/openpi-overlay.dts

Use the following command to compile and install the dts::

    $ sudo dtc -@ -I DTS -O DTB -o /boot/overlays/openpi-overlay.dtb ./openpi-overlay.dts

Support queries
===============
Please use our forums at openmicros.org

http://openmicros.org/index.php/component/kunena/14-openpi
