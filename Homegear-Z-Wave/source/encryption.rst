Encryption
##########

.. highlight:: bash

Homegear supports Z-Wave's S0 mode encryption. To enable it, set the configuration parameter ``password`` to a 16 bytes hexadecimal string in the z-wave.conf file.

Of course, in order to use the secure mode, the device must also support it. Pair the device using secure pairing with ```secpairing on``` in the CLI.


