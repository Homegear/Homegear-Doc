Configuration
#############

.. highlight:: bash

nanoleaf.conf
*************

The configuration file for the Nanoleaf module, ``nanoleaf.conf``, can be found in Homegear's family configuration directory (default: /etc/homegear/families). In this file, you can configure the polling interval used to request state information from the Nanoleafs.

For most installations the default configuration should work fine. By default it looks like this::

	[General]

    moduleEnabled = false

    # Time to wait between each data request from the bridge.
    # Default: 5000
    pollingInterval = 5000


.. note:: By default the module is disabled. To enable it, set ``moduleEnabled`` to ``true``.
