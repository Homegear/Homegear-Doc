Adding HomeMatic BidCoS Devices
###############################

.. highlight:: bash

Make sure the HomeMatioc BidCoS module is up and running before you continue reading this chapter.


Pair a device using the CLI
===========================

Before you start, make sure the device to pair is factory reset if it has been used before.

1. Start the CLI by executing::

	homegear -r

2. Switch to the HomeMatic BidCoS family::

	families select 0

3. Enable the pairing mode in Homegear::

	pairing on

4. Enable pairing mode on the device. See the device's documentation on how to do that.

5. After a few seconds ``ls`` should show the device.
