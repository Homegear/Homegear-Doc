Adding Intertechno Devices
##########################

.. highlight:: bash

Switching Actuators
*******************

To add a switching actuator, you need to know three things:

1. The ID of your communication module as configured in "intertechno.conf" (e. g. "My-CUL").
2. The type ID of the switching actuator to add.
3. The Intertechno address of the switching actuator.

Communication Module ID (1)
===========================

Open the configuration file "intertechno.conf" and search for the line starting with "id =". Write down the ID of the module.


Switching Actuator Type ID (2)
==============================

The type ID of most switching actuators is "1". The only exception are the switching actuators from "REV Ritter". Their type ID is "2".

.. table::
	:column-wrapping: false false

	+--------------------+---------+
	| Switching Actuator | Type ID |
	+====================+=========+
	| REV Ritter         | 2       |
	+--------------------+---------+
	| All other          | 1       |
	+--------------------+---------+


Intertechno Address (3)
=======================

Getting the address is the most tricky part.


New Intertechno Switching Actuators
-----------------------------------

For the new Intertechno switching actuators (the ones without address wheels or DIP switches) just chose an arbitrary but unique number between 1'024 and 67'108'863.


Old Intertechno Switching Actuators
-----------------------------------

The old Intertechno switching actuators (with DIP switch or address wheels) are a little more complicated. First of all determine the 10-digit address with the help of the `fhem Wiki <http://www.fhemwiki.de/wiki/Intertechno_Code_Berechnung>`_. This address needs now to be converted to hexadecimal format. Let's say, your address is "F0FF0FF00F".

* Replace all "F" with "1": F0FF0FF00F => 1011011001
* Convert this binary number into decimal format: 1011011001 => 729

In this case 729 is your address.

For switching actuators from REV Ritter additionally replace all "1" with "0": 1FFF1FF0FF => 0111011011 => 475


.. _adding-device:

Adding the Switching Actuator
=============================

Having the communication module ID, the type ID and the address, adding the device to Homegear is easy.


Adding Devices Using the CLI
----------------------------

To add the device using Homegear's CLI, start it by calling ``homegear -r``. Then execute::

	families select 16
	pc COMMUNICATION_MODULE_ID TYPE_ID ADDRESS

Let's say our communication module ID is "My-CUL" and our address is "788'351" then the command looks like this::

	pc My-CUL 1 788351

The address can be in decimal or hexadecimal format.


Adding Devices Using RPC
========================

You can also add the switching actuator by calling the RPC method ``createDevice()``. The easiest to do so is by executing an inline PHP script on the command line::

	homegear -e rc 'print_v($hg->createDevice(16, 1, "", 788351, -1));'

The first parameter is the family ID, the second the type ID, the fourth is the address. Parameter 3 and 5 are not needed.

Of course you can use all other RPC protocols supported by Homegear to call this method.


Pairing the Switching Actuator
==============================

The new Intertechno switching actuators need to be paired after they are added to Homegear. To do that, you need to plug the actuator to pair in and immediately set ``PAIRING`` on channel 1 to ``true``. To ``PAIRING`` from the command line, execute::

	homegear -e rc '$hg->setValue(<peer ID>, 1, "PAIRING", true);'

Replace ``<peer ID>`` with the ID of the actuator.


Remotes
*******

As with switching actuators to add a remote, you need to know three things:

1. The ID of your communication module as configured in "intertechno.conf" (e. g. "My-CUL").
2. The type ID of the switching actuator to add.
3. The Intertechno address of the switching actuator.

Communication Module ID (1)
===========================

Open the configuration file "intertechno.conf" and search for the line starting with "id =". Write down the ID of the module.


Remote Type ID (2)
==================

New Intertechno Remotes
-----------------------

Find your type ID in the following table. If the number of buttons of your remote is missing, select the next larger one.

+-------------------+---------+
| Remote            | Type ID |
+===================+=========+
| 1-button remote   | 0x10    |
+-------------------+---------+
| 2-button remote   | 0x11    |
+-------------------+---------+
| 3-button remote   | 0x12    |
+-------------------+---------+
| 4-button remote   | 0x13    |
+-------------------+---------+
| 6-button remote   | 0x15    |
+-------------------+---------+
| 8-button remote   | 0x17    |
+-------------------+---------+
| 12-button remote  | 0x1B    |
+-------------------+---------+
| 16-button remote  | 0x1F    |
+-------------------+---------+


Old Intertechno Remotes
-----------------------

Find your type ID in the following table. If your remote is missing, please contact us.

+------------------------------+---------+
| Remote                       | Type ID |
+==============================+=========+
| Original Intertechno remote  | 0x33    |
+------------------------------+---------+
| Elro AB440                   | 0x24    |
+------------------------------+---------+
| b1/Toom                      | 0x24    |
+------------------------------+---------+


Intertechno Address (3)
=======================

As with the switching actuators getting the address is the most tricky part.

New Intertechno Remotes
-----------------------

For the new Intertechno remotes, press a button and watch the Homegear log. The address is logged there::

	10/17/16 16:37:31.228 Intertechno packet received from 012EE0EA (RSSI: -73 dBm): 01

In this case the address is 0x012EE0EA.


Old Intertechno Remotes
-----------------------

The old Intertechno switching actuators (with DIP switch or address wheels) are a little more complicated. The address to set depends on the type of the remote. First of all determine the 10-digit address with the help of the `fhem Wiki <http://www.fhemwiki.de/wiki/Intertechno_Code_Berechnung>`_.


Original Intertechno Remote
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The address has 8 digits. The first 4 are the first 4 digits of your 10-digit code. The last 4 digits depend on the group code:

+---------------+------------------------+--------------------------+
| Rotary Switch | Group Codes            | Last 4 Digits of Address |
+===============+========================+==========================+
| 01 - 04       | 0000, F000, 0F00, FF00 | 0000                     |
+---------------+------------------------+--------------------------+
| 05 - 08       | 00F0, F0F0, 0FF0, FFF0 | 00F0                     |
+---------------+------------------------+--------------------------+
| 09 - 12       | 000F, F00F, 0F0F, FF0F | 000F                     |
+---------------+------------------------+--------------------------+
| 13 - 16       | 00FF, F0FF, 0FFF, FFFF | 00FF                     |
+---------------+------------------------+--------------------------+

So if your 10-digit code is F0FF0FF00F, then the address is F0FF00F0.


Elro AB440 and b1/Toom
^^^^^^^^^^^^^^^^^^^^^^

The address are the first five digits of the 10-digit code. If your 10-digit code is F0FF0FF00F, then the address is F0FF0.


All Remotes
-----------

The address needs now to be converted to hexadecimal format. Let's say, your address is "F0FF00F0".

* Replace all "F" with "1": F0FF00F0 => 10110010
* Convert this binary number into decimal format: 10110010 => 178

In this case 178 is your address.


Adding the Remote
=================

See :ref:`adding-device`.
