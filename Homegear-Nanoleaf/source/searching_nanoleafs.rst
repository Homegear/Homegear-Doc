Searching Nanoleafs
###################

.. highlight:: bash

Make sure the Nanoleaf module is up and running before you continue reading this chapter.

.. _search-devices:

Search for Devices Using the CLI
================================

1. Start the CLI by executing::

	homegear -r

2. Switch to the Nanoleaf family::

	families select 22

3. Search for new Nanoleafs::

	search

4. Now ``ls`` should show all found devices.


.. note:: Homegear never automatically deletes devices. You always have to delete them manually if not needed anymore.
.. note:: IP address changes of Nanoleafs are automatically detected by Homegear.


Search for Devices Using RPC
============================

You can also search for devices by calling the RPC method ``searchDevices()``. The easiest to do so is by executing an inline PHP script on the command line::

	homegear -e rc 'print_v($hg->searchDevices());'

Of course you can use all other RPC protocols supported by Homegear to call this method.
