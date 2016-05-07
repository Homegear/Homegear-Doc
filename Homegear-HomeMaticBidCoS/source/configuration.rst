Configuration
#############

.. highlight:: bash

.. _communication-modules:

Communication Modules
*********************

Overview
========

The HomeMatic BidCoS module supports the following communication modules:

	* :ref:`CUL (busware) <config-cul>`
	* :ref:`COC (busware) <config-coc>`
	* :ref:`CCD (busware) <config-coc>`
	* :ref:`CSM (busware) <config-coc>`
	* :ref:`SCC (busware) <config-coc>`
	* :ref:`CUNX (busware) <config-cunx>`
	* :ref:`HomeMatic Wireless Module for Raspberry Pi (HM-MOD-RPI-PCB, eQ-3) <config-hm-mod-rpi-pcb>`
	* :ref:`HomeMatic LAN Configuration Adapter (HM-CFG-LAN, eQ-3) <config-hm-cfg-lan>`
	* :ref:`HomeMatic LAN Gateway (HM-LGW, eQ-3): Crashes sometimes <config-hm-lgw>`
	* :ref:`HomeMatic USB Configuration Adapter (HM-CFG-USB(-2), eQ-3) <config-hm-cfg-usb>`
	* :ref:`CC1101 (Texas Instruments) <config-ticc1101>`
	* :ref:`CC1101 with CC1190 (Texas Instruments) <config-ticc1101-cc1190>`

If you just want a system that works without you having to invest a lot of time, buy the CUL stick. Of the devices in the list, it is probably the easiest to use. Additionally, it can be connected to a good antenna and supports AES handshakes and firmware updates.

.. note:: Of course, you can use multiple communication modules at the same time.

The HomeMatic BidCoS communication modules need to be configured in ``homematicbidcos.conf``. This file needs to be placed into Homegear's family configuration directory (by default /etc/homegear/families).

.. _config-cul:

CUL
===

.. image:: images/cul.jpg

The CUL from `busware <http://busware.de/tiki-index.php?page=CUL>`_ is the most easy to use hardware to communicate with HomeMatic BidCoS devices. The only disadvantage is the bad control over the packet timing due to the use of USB.

Downloading the Prerequisites
-----------------------------

In order to be able to flash the CUL you need to install dfu-programmer. In Debian just run::

	apt-get install dfu-programmer

Flashing the Firmware
---------------------

Download the firmware from `culfw.de <http://culfw.de/>`_ and extract it::

	wget http://culfw.de/culfw-1.58.tar.gz
	tar -zxf culfw-1.58.tar.gz

Change to the directory with the CUL firmware::

	cd CUL_VER_*/culfw/Devices/CUL

Now press the PROGRAM button on the back side of your CUL and keep it pressed while plugging the CUL in. The green LED should NOT flash. Then execute::

	dfu-programmer atmega32u4 erase
	dfu-programmer atmega32u4 flash CUL_V3.hex
	dfu-programmer atmega32u4 reset

Plug out and plug in your CUL again and you are done!

Configuring Homegear to Use the CUL
-----------------------------------

To tell Homegear to use the CUL place these lines into that file::

	[CUL]
	deviceType = cul
	device = /dev/ttyACM0
	responseDelay = 95

.. _config-coc:

COC/CCD/SCC
===========

.. image:: images/coc.jpg

The COC from `busware <http://busware.de/tiki-index.php?page=CUL>`_ is a Raspberry Pi extension to communicate with wireless home automation devices. Because the communication between COC and Raspberry Pi is serial, the packet timing is much better than with a CUL.

Downloading the Prerequisites
-----------------------------

In order to be able to flash the COC you need to install avrdude. In Debian just run::

	apt-get install avrdude


Free Up Serial Line
-------------------

Remove any references to ttyAMA0 from /etc/inittab and /boot/cmdline.txt.

My /boot/cmdline.txt looks like this::

	dwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait

And the last lines of my /etc/inittab (I just added the comment in front of the last line)::

	#T3:23:respawn:/sbin/mgetty -x0 -s 57600 ttyS3
	 
	 
	#Spawn a getty on Raspberry Pi serial line
	#T0:23:respawn:/sbin/getty -L ttyAMA0 115200 vt100

Reboot the Raspberry Pi. 


Flashing the Firmware
---------------------

Download the firmware from culfw.de and extract it::

	wget http://culfw.de/culfw-1.58.tar.gz
	tar -zxf culfw-1.58.tar.gz

Change to the directory with the COC firmware::

	cd CUL_VER_*/culfw/Devices/COC

Then execute (just copy and paste the commands)::

	if test ! -d /sys/class/gpio/gpio17; then echo 17 > /sys/class/gpio/export; fi
	if test ! -d /sys/class/gpio/gpio18; then echo 18 > /sys/class/gpio/export; fi
	echo out > /sys/class/gpio/gpio17/direction
	echo out > /sys/class/gpio/gpio18/direction
	echo 0 > /sys/class/gpio/gpio18/value
	echo 0 > /sys/class/gpio/gpio17/value
	sleep 1
	echo 1 > /sys/class/gpio/gpio17/value
	sleep 1
	echo 1 > /sys/class/gpio/gpio18/value
	 
	avrdude -p atmega1284p -P /dev/ttyAMA0 -b 38400 -c avr109 -U flash:w:COC.hex


Configuring Homegear to Use the COC/CCD/CSM/SCC
-------------------------------------------

To tell Homegear to use the CUL place these lines into that file::

	[COC/CCD/CSM/SCC]
	deviceType = coc
	device = /dev/ttyAMA0
	responseDelay = 95
	gpio1 = 17
	gpio2 = 18
	# Set stackPositition if you use stacking (starting with "1" for the SCC at the bottom).
	# stackPosition = 1

If you want to stack multiple SCC, you need to set "stackPosition". Use "1" for the SCC at the bottom, "2" for the second SCC, "3" for the next one and so on.

.. _config-cunx:

CUNX
====

Just connect it ;). Of course you can use multiple CUNXs.

Configuring Homegear to Use the CUNX
------------------------------------

To tell Homegear to use the CUNX place these lines into that file::

	[CUNX]
	id = My-CUNX
	## Uncomment this if you want this CUNX to be your default communication module.
	#default = true
	deviceType = cunx
	## IP address of your CUNX
	host = 192.168.178.100
	port = 2323
	responseDelay = 93

.. _config-hm-mod-rpi-pcb:

HomeMatic Wireless Module for Raspberry Pi (HM-MOD-RPI-PCB)
===========================================================

::

	[HomeMatic Wireless Module for Raspberry Pi]
	id = My-HM-MOD-RPI-PCB
	## Uncomment this if you want the HM-MOD-RPI-PCB to be your default communication module.
	#default = true
	deviceType = hm-mod-rpi-pcb
	device = /dev/ttyAMA0
	responseDelay = 95
	gpio1 = 18

.. _config-hm-cfg-lan:

HomeMatic LAN Configuration Adapter (HM-CFG-LAN)
================================================

.. _config-hm-lgw:

HomeMatic LAN Gateway (HM-LGW)
==============================

.. _config-hm-cfg-usb:

HomeMatic USB Configuration Adapter (HM-CFG-USB[-2])
====================================================

.. _config-ticc1101:

CC1101
======

.. _config-ticc1101-cc1190:

CC1101 with CC1190
==================