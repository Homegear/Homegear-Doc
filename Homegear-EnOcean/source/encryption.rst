Encryption
##########

.. highlight:: bash

Homegear supports EnOcean's Rolling Code encryption. To enable it, set the configuration parameter ``ENCRYPTION`` to ``true``, e. g. by executing on the console::

    homegear -e rc '$hg->putParamset(<Peer ID>, 0, array("ENCRYPTION" => true));'

Then send a teach-in packet from the device. Please consult the device's manual on how to do that. After the packet has been sent, the Homegear log should show a line like::

    Module EnOcean: Info: Encryption was setup successfully. Encryption type: 3, CMAC size: 3 bytes, Rolling Code size: 2 bytes, Rolling Code: 0xED3C, Key: AE7B242C037FE6F098C8AEAE04CF1F5F

Now encryption is enabled.