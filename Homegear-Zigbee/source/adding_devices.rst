Adding Zigbee Devices
#####################

.. highlight:: bash

Make sure the Zigbee module is up and running before you continue reading this chapter.


Pair a device using the CLI
===========================

Before you start, make sure the device to pair is factory reset if it has been used before.

1. Start the CLI by executing::

	homegear -r

2. Switch to the Zigbee family::

	families select 26

3. Enable the pairing mode in Homegear::

	pairing on

4. Enable pairing mode on the device. See the device's documentation on how to do that. Typically it's a press on a button of the device.

5. After a few seconds ``ls`` should show the device. Please wait some more seconds until the device is fully queried and configured. Battery devices might go to sleep before the whole process is finished, you may have to keep them awake by pressing the button on the device several seconds apart.

.. note:: You can also pair a device by using the command: ``homegear -e rc '$hg->setInstallMode(26, true, 60);'``
