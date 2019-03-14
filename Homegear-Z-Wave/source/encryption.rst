Encryption
##########

.. highlight:: bash

Homegear supports Z-Wave's S0 mode encryption. To enable it, set the configuration parameter ``password`` to a 16 bytes hexadecimal string in the z-wave.conf file.
Homegear also supports Z-Wave's S2 mode encryption. To enable it, set the configuration parameters ``passwordS21``, ``passwordS22`` and ``passwordS23`` each to a 16 bytes hexadecimal string in the z-wave.conf file. The passwords need to be different.

Of course, in order to use the secure mode, the device must also support it. Pair the device using S0 secure pairing with ```secpairing on``` in the CLI.
Pair the device using S2 secure pairing with ```sec2pairing3 on``` in the CLI. You may replace 3 with the maximum security level you want to grant to the device. This command will also grant (if the device requests them) all lower security levels to the device. This is useful if you want to pair the device with some other device that supports a lower security level mode. Homegear will use the maximum granted level to communicate with the device.
If you want to grant only a single security level, use ```sec2pairing3e on```. 'Access Control mode' corresponds to 3, 'Authenticated mode' corresponds to 2, 'Unauthenticated mode' corresponds to 1 and 0 corresponds to 'S0 legacy mode'. 'Access Control' and 'Authenticated' modes need to specify the five digits start of the DSK you may find on a label on the back of the device, or inside, in the battery compartment.
