Configuration
#############

.. highlight:: bash

m-bus.conf
**********

The configuration file for the M-Bus module, ``m-bus.conf``, can be found in Homegear's family configuration directory (default: ``/etc/homegear/families``). In this file, you can configure the Amber wireless transceiver and the allowed security modes. Both the Amber USB modules and the serial modules are supported.

Here's an example configuration::

	[General]

	moduleEnabled = true

	# Comma-seperated list of allowed security modes

	securityModeWhitelist = 7

	[Amber Wireless Transceiver]

	# Specify an unique id here to identify this device in Homegear
	# After devices are paired to Homegear don't rename the interface
	# as the ID is used to assign it to the peers!
	id = My-Amber

	# Options: amber
	deviceType = amber

	# Device name of your interface
	device = /dev/ttyUSB0

	# The baudrate of the Amber module
	baudrate = 9600

	# The M-Bus mode (C, T, or S)
	mode = c


.. note:: We recommend M-Bus transmission mode "C" as it requires the least amount of energy.

.. warning:: The BSI requires at least security mode 7. For security reasons don't enable other security modes unless absolutely needed.
