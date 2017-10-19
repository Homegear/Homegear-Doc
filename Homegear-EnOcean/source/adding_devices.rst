Adding EnOcean Devices
######################

.. highlight:: bash

With EnOcean there are three methods to add devices. See :ref:`supported-devices` to find out, what method to use.


.. _sniffing-packets:

Sniff Packets to Find EnOcean ID
********************************

If you don't know the EnOcean ID of the device you want to add to Homegear and the device's pairing method is "Create Device" or "Manual Teach-in", you can sniff for devices packets. To start sniffing execute on the console::

    homegear -e rc '$hg->startSniffing(15);'


If possible, trigger the sending of a packet on the device. If not, you need to wait, until the device automatically sends a packet. Then execute::

    homegear -e rc '$hg->getSniffedDevices(15);'


This will output the EnOcean ID (= address), RORG (first two letters [= first byte] of the EEP), the RSSI and the time of the received packet.

When done, execute::

    homegear -e rc '$hg->stopSniffing(15);'


.. _create-device:

Create Device
*************

Create device is the pairing method for most sensors. For it to work you need to know the EnOcean Equipment Profile (EEP) of the device and it's EnOcean ID (= address). If you don't know the EnOcean ID, you can sniff it (see :ref:`sniffing-packets`).

To create the device, execute on the console::

    homegear -e rc 'print_v($hg->createDevice(15, hexdec("<EEP>"), "", hexdec("<EnOcean ID>"), 0, <Interface ID>));'


The interface ID only needs to be set if multiple communication modules are available. It is defined in the EnOcean configuration file (by default in "/etc/homegear/families/enocean.conf"). If the EEP is A50201 and the EnOcean ID FA087403, the command is::

    homegear -e rc 'print_v($hg->createDevice(15, hexdec("A50201"), "", hexdec("FA087403"), 0, <Interface ID>));'


When the command executed successfully, the new peer's ID is returned.


.. _manual-teach-in:

Manual Teach-in
***************

This pairing method is used by older actuators. Start by following the instructions in :ref:`create-device`. Then you need to enable the pairing mode on the device according to it's manual. After that, you need to set the variable ``PAIRING`` for the channel you want to pair (most devices only have one channel). ``PAIRING`` needs to be set to a value between 0 and 127. You need to choose a unique value per communication interface per device if you want to control the devices seperately. If you use the same ID for two devices, these devices are grouped together. To set pairing on the console, execute::

    homegear -e rc '$hg->setValue(<Peer ID>, <Channel>, "PAIRING", <Unique ID>);'


When the peer ID is 29, the channel is 1 and the unique ID is 0, the command is::

    homegear -e rc '$hg->setValue(29, 1, "PAIRING", 0);'


Now the device should be successfully paired to Homegear.


.. _pairing:

Pairing
*******

This is the easiest pairing method. In Homegear just enable the pairing mode::

    homegear -e rc '$hg->setInstallMode(15, true, 60);'


Now enable the pairing mode on the device according to it's manual. That should be it. You can see if the device was successfully paired in Homegear's CLI::

    homegear -r
    families select 15
    ls