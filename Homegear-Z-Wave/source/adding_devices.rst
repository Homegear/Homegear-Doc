Adding Z-Wave Devices
#####################

.. highlight:: bash

Make sure the Z-Wave module is up and running before you continue reading this chapter.


Pair a device using the CLI
===========================

Before you start, make sure the device to pair is factory reset if it has been used before.

1. Start the CLI by executing::

	homegear -r

2. Switch to the Z-Wave family::

	families select 17

3. Enable the pairing mode in Homegear::

	pairing on

 or if you want to pair it in S0 secure mode::

	secpairing on

 The device must support S0 security for the later case.

 If you want the device paired in S2 mode, use either::

	sec2pairing3 on

 where 3 may be replaced with the maximum security level you want to grant, or::

	sec2pairing3e on

 in which case only one security level is granted (if the device supports it). Of course, the device must support the S2 security levels in order to be able to grant them. Please check the encryption section for more details about security modes.

4. Enable pairing mode on the device. See the device's documentation on how to do that. Typically it's a triple-click (or a single or a double-click) on a button of the device.

5. After a few seconds ``ls`` should show the device.

.. note:: You can also pair a device by using the command: ``homegear -e rc '$hg->setInstallMode(17, true, 60);'``

.. note:: By default devices are added in low power mode, this is a security measure, important when you add devices in secure mode. You should have the added device as close as possible to the communication device. If that is not possible, you can enable 'high power' mode from CLI.
