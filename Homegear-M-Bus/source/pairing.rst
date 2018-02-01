Pairing M-Bus Devices
#####################

.. highlight:: bash

Make sure the M-Bus module is up and running before you continue reading this chapter.


Pair Devices Using RPC
======================

You can pair new devices by calling the RPC method ``setInstallMode()``. The easiest to do so is by executing an inline PHP script on the command line::

	homegear -e rc '$hg->setInstallMode(23, true, array("devices" => array(array("address" => hexdec("74600338"), "key" => "00112233445566778899AABBCCDDEEFF"))));'

.. highlight:: json

The first parameter ``23`` is the family ID of the M-Bus module. ``true`` tells Homegear to enable the pairing mode. The last parameter is an array with the following structure::

    {
      "devices":
      [
        {
          "address": hexToDec(64656081),
          "key": "00112233445566778899AABBCCDDEEFF" [optional]
        },
        {
          "address": hexToDec(64656082),
          "key": "00112233445566778899AABBCCDDEEFF" [optional]
        },
        .
        .
        .
      ]
    }

.. highlight:: bash

This structure tells Homegear which devices are to be paired. It is stored in the database so the devices can even be paired after a restart of Homegear. I. e. Homegear can be preconfigured for an installation. The address is typically the serial number printed on the device (if you don't know the address, watch the Homegear log on at least log level 4 - the address is logged there). This serial number is interpreted as a hexadecimal number, so it needs to be converted to a decimal number first. Let's say the serial number is "64656081" then the address to pass in the structure is "1684365441". The ``hexdec()`` above does exactly this conversion. The key needs to be set for encrypted packets, otherwise they can't be interpreted.

If you want to disable pairing, call ``setInstallMode()`` with ``false`` as second parameter or an empty structure as the third parameter::

    homegear -e rc '$hg->setInstallMode(23, false);'

Of course you can use all other RPC protocols supported by Homegear to call this method.
