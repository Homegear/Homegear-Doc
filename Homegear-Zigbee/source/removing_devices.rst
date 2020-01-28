Removing Zigbee Devices
=======================

.. highlight:: bash

Zigbee devices can be removed using the Command Line Interface (CLI). They are never removed automatically by Homegear.


Remove Device Using CLI
***********************

To remove a device with Homegear's CLI, start it by calling ``homegear -r``. Then execute::

	families select 26
	ron 1

Of course, you should replace 1 with the peer id you want to remove.

You'll have to put the device you want to remove in removal mode, this is dependent upon device, please consult the device manual. It might be a single press on a button of the device.
