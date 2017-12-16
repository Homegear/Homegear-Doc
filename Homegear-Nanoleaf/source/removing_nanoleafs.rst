Removing Nanoleafs
##################

.. highlight:: bash

Nanoleafs can either be removed using the Command Line Interface (CLI) or the RPC method ``deleteDevice()``. They are never removed automatically by Homegear. So if a Nanoleaf doesn't exist anymore you need to delete it manually.


Remove Device Using CLI
***********************

To remove a device with Homegear's CLI, start it by calling ``homegear -r``. Then execute::

	families select 22
	peers remove PEERID

The peer ID can be retrieved by calling ``ls`` before executing ``peers remove``.


Remove Device Using RPC
***********************

The simplest way to call ``deleteDevice()`` is by placing it into an inline PHP script. The argument ``flags`` is completely ignored and can be set to ``0``. To for example delete the device with peer ID 16 run on the command line::

	homegear -e rc '$hg->deleteDevice(16, 0);'