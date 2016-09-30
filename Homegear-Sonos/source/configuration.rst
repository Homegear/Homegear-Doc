Configuration
#############

.. highlight:: bash

sonos.conf
**********

The configuration file for the Sonos module, ``sonos.conf``, can be found in Homegear's family configuration directory (default: /etc/homegear/families). In this file, you can configure the event server listening for packets from the Sonos speakers and Ivona TTS.

Event Server
************

For most installations the default configuration should work fine. By default it looks like this::

	[Event Server]
	id = My-Sonos-1234
	deviceType = eventserver
	#host =
	#port = 7373
	ttsProgram = homegear -e rs DeviceScripts/Sonos/IvonaTTS.php

``host`` can be set to the ip address the event server should listen on, if it cannot be automatically determined. The default ``port`` is 7373. If necessary this can be changed as well.


TTS
***

``ttsProgram`` is set to an Ivona script by default. Currently this is the only easy to use script available. This script needs two options which can be defined in the ``[General]`` section of ``sonos.conf``:

* ``ivonaTtsAccessKey``: The access key (the short one) which you get after signing up for the Ivona TTS Cloud.
* ``ivonaTtsSecretKey``: The secret key (the long one) which you get after signing up for the Ivona TTS Cloud.

.. warning:: Note that it takes a while after signing up until access and secret key work.

.. note:: If you develop your own user interface, ``ivonaTtsAccessKey`` and ``ivonaTtsSecretKey`` can alternatively be set using the RPC method ``setFamilySetting``. This overwrites the settings in ``sonos.conf``.
