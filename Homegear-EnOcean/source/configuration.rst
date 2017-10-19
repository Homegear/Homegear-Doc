Configuration
#############

.. highlight:: bash

enocean.conf
************

The configuration file for the EnOcean module, ``enocean.conf``, can be found in Homegear's family configuration directory (default: /etc/homegear/families). In this file, you can configure the communication modules used to communicate with EnOcean devices.


Communication Modules
*********************

Overview
========

The EnOcean module supports all communication modules using the TCM310:

	* EnOcean USB 300
	* EnOcean Pi Wireless Module

We recommend using a module with a good antenna as this greatly increases the range. The chip antenna on the USB 300 is definitely not recommended.


TCM310
======

To tell Homegear to use the module, insert the following lines into ``enocean.conf``::

	[TCM310]
    id = TCM310
    deviceType = usb300
    device = /dev/ttyS0

Of course, you can use multiple communication modules with Homegear.