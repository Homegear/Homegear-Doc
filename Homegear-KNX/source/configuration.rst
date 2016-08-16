Configuration
#############

.. highlight:: bash

knx.conf
********

The configuration file for the KNX module, ``knx.conf``, can be found in Homegear's family configuration directory (default: /etc/homegear/families). In this file, you can configure the communication modules you want to use.

.. _communication-modules:

Communication Modules
*********************

Overview
========

The KNX module can use all KNX IP gateways and KNX IP routers supporting the KNXnet/IP tunneling protocol (= EIBnet/IP tunneling protocol, = IP tunneling protocol - there is only one).

.. note:: Of course, you can configure multiple communication modules at the same time.

.. _config-knx-ip-tunneling:

Configuring an IP Gateway/Router
================================

To tell Homegear to use your IP gateway, insert these lines into ``knx.conf``::

	[KNXnet/IP]
	id = My-KNX-gateway
	deviceType = knxnetip
	# IP address of your gateway
	host = 192.168.178.100
	port = 3671
	# Listen IP of the computer running Homegear. Leave empty to autodetect.
	# listenIp =
	# Port Homegear listens on for packets from your KNXNet/IP interface
	listenPort = 5671
