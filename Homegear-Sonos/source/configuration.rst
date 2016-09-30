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


Test TTS
========

First determine the peer ID of the speaker you want to output TTS on. The speaker needs to be a master, i. e. it must not be paired as a slave to another speaker. Let's assume the peer ID is 16, then to test TTS execute the following commands in Homegear's CLI::

	# For English
	$hg->setValue(16, 1, "PLAY_TTS_LANGUAGE", "en");
	$hg->setValue(16, 1, "PLAY_TTS", "Hello world");

	# For German
	$hg->setValue(16, 1, "PLAY_TTS_LANGUAGE", "de");
	$hg->setValue(16, 1, "PLAY_TTS", "Hallo Welt");

There are two more TTS variables:

* ``PLAY_TTS_VOLUME``: This sets the volume used for TTS output. If set to ``-1``, the currently set volume is used.
* ``PLAY_TTS_UNMUTE``: If set to ``true`` and the speakers are muted, they are unmuted before playing the TTS.