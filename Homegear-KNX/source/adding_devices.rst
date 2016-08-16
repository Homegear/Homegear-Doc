Adding KNX Devices
##################

.. highlight:: bash

Make sure the KNX module is up and running before you continue reading this chapter.


Map Each Group Address to One Homegear Device
*********************************************

The following steps show you how to add KNX group addresses to Homegear. Each group address is mapped to one Homegear device. To learn how to group multiple KNX group addresses together into one Homegear device follow the steps in section :ref:`grouped-addresses`.

1. Export your KNX project file (file ending .knxproj).
2. Place this file in Homegear's KNX device description directory. By default this is ``/etc/homegear/devices/14/``.

Now there are two ways to add the devices. Either by using Homegear's Command Line Interface (CLI) or by calling the RPC method ``searchDevices()``.


.. _search-devices:

Search for Devices Using the CLI
================================

1. Start the CLI by executing::

	homegear -r

2. Switch to the KNX family::

	families select 14

3. Search for new KNX devices::

	search

4. Now ``ls`` should show all found devices.

If a group address was already known to Homegear the Homegear device is updated. There is no need to previously delete the Homegear device.

.. note:: Homegear never automatically deletes devices. You always have to delete them manually if not needed anymore.


Search for Devices Using RPC
============================

You can also search for devices by calling the RPC method ``searchDevices()``. The easiest to do so is by executing an inline PHP script on the command line::

	homegear -e rc 'print_v($hg->searchDevices());'

Of course you can use all other RPC protocols supported by Homegear to call this method.


.. _grouped-addresses:

Multiple KNX Group Addresses in One Homegear Device
***************************************************

The following steps show you how to add multiple KNX group addresses into one Homegear device. There is no way to group addresses together automatically in a way that makes sense, so this step needs to be done manually by adding meta information to the group variables in ETS. The meta data needs to be added in the description field of the group variables. It needs to be formatted as a JSON object starting with ``$${``. Around the JSON object any other text is still allowed in the description field.

.. figure:: images/group-variable-metadata.png
	:width: 300px

	Description field with JSON metadata in ETS 5.

The following keys are available:

+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+
| Key       | JSON type | Description                                                                                                       | Example       | Default |
+===========+===========+===================================================================================================================+===============+=========+
| id        | string    | The ID of the Homegear device. All devices with the same ID are grouped together.                                 | "Main switch" | ""      |
+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+
| type      | integer   | The type ID of the device. Needs to unique per device. You only need to specify it once for each Homegear device. | 5             | 0       |
+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+
| channel   | integer   | The channel number the group variable should be placed in.                                                        | 1             | 1       |
+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+
| variable  | string    | The name the group variable should have in Homegear.                                                              | "STATE"       | "VALUE" |
+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+
| unit      | string    | The unit (m², l, h, °C, ...) that should be displayed in Homegear.                                                | "°C"          | ""      |
+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+
| readable  | boolean   | Set to "false" to mark variable as "write only".                                                                  | false         | true    |
+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+
| writeable | boolean   | Set to "false" to mark variable as "read only"                                                                    | false         | true    |
+-----------+-----------+-------------------------------------------------------------------------------------------------------------------+---------------+---------+

All group variables with the same value for ``id`` are grouped together. For each device a unique number for ``type`` needs to be specified. Each variable needs a unique value for ``variable`` within one channel. All other keys are optional.

Example
=======

Let's say you have two push buttons with two switchable status LEDs you want to group together into one Homegear device. The id of the device should be "My Push Buttons", the type number "4215" (you can choose any unique value you like). The names of the push button state variables should be "PRESS" and the name of the status LED variables should be "LED_STATE". "PRESS" should be read only. The variables of the first push button should be placed into channel 1 and the variables of the second push button into channel 2. Then the text you need to place into the ETS description fields of the four variables is:

+---------------------------+--------------------------------------------------------------------------------------------------+
| KNX group variable        | JSON object                                                                                      |
+===========================+==================================================================================================+
| Push button 1 "PRESS"     | $${"id": "My Push Buttons", "type": 4215, "channel": 1, "variable": "PRESS", "writeable": false} |
+---------------------------+--------------------------------------------------------------------------------------------------+
| Push button 1 "LED_STATE" | $${"id": "My Push Buttons", "channel": 1, "variable": "LED_STATE"}                               |
+---------------------------+--------------------------------------------------------------------------------------------------+
| Push button 2 "PRESS"     | $${"id": "My Push Buttons", "channel": 2, "variable": "PRESS", "writeable": false}               |
+---------------------------+--------------------------------------------------------------------------------------------------+
| Push button 2 "LED_STATE" | $${"id": "My Push Buttons", "channel": 2, "variable": "LED_STATE"}                               |
+---------------------------+--------------------------------------------------------------------------------------------------+

Now follow the steps in section :ref:`search-devices`.


Variable Representation in Homegear
***********************************

The KNX datapoint type is converted to an appropriate type in Homegear. Some KNX datapoint types are too complex to represent them in one variable. In this case, it is split into multiple variables. Let's say the name of the complex variable is "MY_VAR". Then every variable will start with "MY_VAR" followed by a "." and a subvariable name. The raw value can by accessed through "MY_VAR.RAW". The subvariable values can be accessed by datapoint specific names. Subvariable values can be set like any other variable. But to send the changes you need to call "MY_VAR.SUBMIT".


Example:
========

The datapoint type is "DPT-30 (DPST-30-1010)". The variable name is "STATES". The peer ID is 161. The channel is 1. In this case there will be 26 variables. "STATES.RAW", "STATES.STATE_1" to "STATES.STATE_24" and "STATES.SUBMIT". To set "STATES.STATE_5" to "true" and "STATES.STATE_10" to "false" with inline PHP execute on the command line::

	homegear -e rc '$hg->setValue(161, 1, "STATES.STATE_5", true);'
	homegear -e rc '$hg->setValue(161, 1, "STATES.STATE_10", false);'
	homegear -e rc '$hg->setValue(161, 1, "STATES.SUBMIT", true);'

Alternatevily you could've set "STATES.RAW"::

	homegear -e rc '$hg->setValue(161, 1, "STATES.RAW", hexdec("80000"));'