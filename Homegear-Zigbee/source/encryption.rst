Encryption
##########

.. highlight:: bash

Homegear supports Zigbee encryption through the builtin support in the firmware of the communication device. To set it, set the configuration parameter ``password`` to a 16 bytes hexadecimal string in the zigbee.conf file.

This is for the Zigbee network itself, for other communications (such as the gateway), homegear provides the same security as for the other modules.