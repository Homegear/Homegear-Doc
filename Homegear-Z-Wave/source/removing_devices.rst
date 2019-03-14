Removing Z-Wave Devices
=======================

.. highlight:: bash

Z-Wave devices can be removed using the Command Line Interface (CLI). They are never removed automatically by Homegear.


Remove Device Using CLI
***********************

To remove a device with Homegear's CLI, start it by calling ``homegear -r``. Then execute::

	families select 17
	remove on

You'll have to put the device you want to remove in removal mode, this is dependent upon device, please consult the device manual. It might be a triple-click or just a single-click on a button of the device. Sometimes you have to push and hold several buttons of the device.


Remove a Failed Device Using CLI
********************************

To remove a failed device with Homegear's CLI, start it by calling ``homegear -r``. Then execute::

	families select 17
	remove failed on
