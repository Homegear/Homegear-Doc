.. _supported-devices:

Supported Devices
#################

.. highlight:: bash

Homegear supports all newer Zigbee 3 devices. It also supports 'legacy' devices with the proper setting in zigbee.conf. There might be some old devices that are not supported.

We tested the Zigbee module with various usb sticks and for raspberry pi, even uart devices, based on chips supporting TI Z-Stack. Some that work are with CC2538 or CC2530, for example. 
Be aware that there are many firmwares available for such devices, some are built with settings that do not allow using the full functionality. One we tested lost its settings each time it lost power, entering automatically in network commissioning mode after it was repowered.

The Zigbee module supports the Zigbee devices in a generic way. We tested the Zigbee module with many devices from many manufacturers, but there still can be devices out there that do not work, or work only partially (especially some older ones). If you find one, please contact us.

