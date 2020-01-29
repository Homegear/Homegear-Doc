Configuration
#############

.. highlight:: bash

.. note:: If you used the communication module with some other software and added devices to it, you need first to unpair the devices with that software and maybe even reset the communication module. Alternatively you may reset the devices manually and also reset the communication device from homegear.

zigbee.conf
***********

The configuration file for the Zigbee module, ``zigbee.conf``, can be found in Homegear's family configuration directory (default: ``/etc/homegear/families``). In this file, you can configure the communication modules used to communicate with Zigbee devices.

Communication Modules
*********************

Overview
========

The Zigbee module supports TI Z-Stack 3.0.x communication modules using the USB interface.

On Raspberry Pi it also supports communication modules that use the uart interface.

The Zigbee module can also connect to a remote usb stick or uart communication module using the Homegear Gateway service:

    * :ref:`Homegear Gateway <config-homegear-gateway>`


.. serial:

Serial
======

To tell Homegear to use the usb module, insert the following lines into ``zigbee.conf``::

	[Serial]

	id = Serial1
	deviceType = serial

	#use your own 16 bytes hexadecimal key!
	password = 16CFA1797F981EC8651DDD45F8BF0FC6

	device = /dev/ttyUSB1


Of course, you can use multiple communication modules with Homegear. We recommend to use the gateway (see below) in such a case, but you could use more than one local device. The downsize is that you have to pull out the usb devices except one when you add a zigbee node, because pairing activates all interfaces.

For USB devices this is all. In case you are using an UART device on the Raspberry Pi, additionally follow these steps:


Free Up Serial Line and Enable UART
-----------------------------------

All Raspberry Pis
^^^^^^^^^^^^^^^^^

ttyAMA0 or serial0 might be used by the serial console. To free it up do the following.

Remove any references to ttyAMA0 and serial0 from /etc/inittab and /boot/cmdline.txt.

Our /boot/cmdline.txt looks like this::

    dwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait

You may also use ``raspi-config`` to disable console output on the serial line.

Raspberry Pi 3
^^^^^^^^^^^^^^

On the Raspberry Pi 3 /dev/ttyAMA0 is used by the Wifi and Bluetooth module. There is a "mini UART" available on /dev/ttyS0 by default. It is better though, to use the hardware UART and switch the Wifi/Bluetooth module to mini UART. To do that, add this line at the end of ``/boot/config.txt``::

    dtoverlay=pi3-miniuart-bt

Alternatively you might try this::

    dtoverlay=pi3-disable-bt

Additionally remove any references to ttyAMA0 from ``/boot/cmdline.txt``. Our file looks like this::

    dwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait


All Raspberry Pis
^^^^^^^^^^^^^^^^^

Make sure ``enable_uart=1`` is in ``/boot/config.txt``. Our file looks like this::

    .
    .
    .
    enable_uart=1
    dtparam=spi=on
    dtparam=i2c_arm=on

Disable the serial interface in Raspbian Jessie::

    systemctl disable serial-getty@ttyAMA0.service
    systemctl disable serial-getty@serial0.service
    systemctl disable serial-getty@ttyS0.service

Reboot the Raspberry Pi.

.. warning:: If you're using the official Raspbian, you need to comment the lines containing "gpio" in file ``/etc/udev/rules.d/99-com.rules`` (place a "#" at the beginning of the lines) for Homegear to be able to access the GPIOs.


.. _config-homegear-gateway:

Homegear Gateway
================

Certificate Generation
----------------------

First you need to create certificates for the Gateway service. We don't want an insecure gateway so there is no possibility to use it without creating them. If not done already, start by following the instructions `to create a certificate authority in the Homegear manual <https://doc.homegear.eu/homegear/installation.html#create-homegear-s-certificate-authority>`_.

First create the gateway certificates using Homegear Management::

    homegear -e rc 'print_v($hg->managementCreateCert("my-gateway"));'

Replace ``my-gateway`` with an arbitrary name (it doesn't need to be the hostname of the gateway). The name will be used to set the field ``COMMON NAME`` of the certificate. It has to be the same as set to the setting ``id`` in ``zigbee.conf`` (see below).

The output of the command looks similar to::

    (Struct length=5)
    {
      [caPath]
      {
        (String) /etc/homegear/ca/cacert.pem
        {
          [certPath]
          {
            (String) /etc/homegear/ca/certs/zigbee-gateway-01.crt
          }
          [commonNameUsed]
          {
            (String) zigbee-gateway-01
          }
          [filenamePrefix]
          {
            (String) zigbee-gateway-01
          }
          [keyPath]
          {
            (String) /etc/homegear/ca/private/zigbee-gateway-01.key
          }
        }
      }
    }

In case your chosen name contained invalid characters, ``commonNameUsed`` returns the corrected name that will be used in the certificate. ``certPath`` is the path Homegear tries to create the certificate in, ``keyPath`` the path to the private key file. The actual certificate generation starts in background. To check if the command has finished, execute::

    homegear -e rc 'print_v($hg->managementGetCommandStatus());'

This returns the command output and the exit code. The command has finished if the exit code is other than ``256``. On success the exit code is ``0``.


Find Gateways
-------------

If you don't know the IP address of your gateway, you can search and print all unconfigured gateways with the following command::

    homegear -e rc '$devices=$hg->ssdpSearch("urn:schemas-upnp-org:device:basic:1", 5000);foreach($devices as $device){if(!array_key_exists("additionalFields", $device) || !array_key_exists("hg-family-id", $device["additionalFields"]) || !array_key_exists("hg-gateway-configured", $device["additionalFields"])) continue; if($device["additionalFields"]["hg-family-id"] != "15" || $device["additionalFields"]["hg-gateway-configured"] != "0") continue; print($device["ip"].PHP_EOL);}'


Homegear Gateway Service
------------------------

If you have a preconfigured Homegear Gateway you can skip this section. This section covers the installation of the Homegear Gateway service. First setup a computer with Debian, Raspbian or Ubuntu and connect a serial communication module or USB stick.

Add the Homegear APT repository and install Homegear Gateway::

    apt install homegear-gateway


Open ``/etc/homegear/gateway.conf`` and set the settings for your communication module, e. g. for an USB stick on device ``/dev/ttyUSB1``::

    family = zigbee
    device = /dev/ttyUSB1

Note the ``configurationPassword``, we need below.

Restart the gateway service::

    service homegear-gateway restart


Check ``/var/log/homegear-gateway/homegear-gateway.log`` for errors. If everything is working, the logfile should say ``Startup complete`` and print a warning that the gateway is unconfigured.

.. note:: To reset a gateway (make it "unconfigured"), delete the files ``<dataPath>/ca.crt``, ``<dataPath>/gateway.crt`` and ``<dataPath>/gateway.key``. ``dataPath`` is configured in ``/etc/homegear/gateway.conf``.


Homegear
--------

To configure a gateway, execute::

    homegear -e rc '$hg->configureGateway("<IP>", 2018, file_get_contents("/etc/homegear/ca/cacert.pem"), file_get_contents("/etc/homegear/ca/certs/<your-cert>.crt"), file_get_contents("/etc/homegear/ca/private/<your-cert>.key"), "<your-configuration-password>");'

Replace ``<your-cert>`` with the value of ``commonNameUsed`` from above, ``<IP>`` with the IP address of your gateway and ``<your-configuration-password>`` with ``configurationPassword`` from the ``gateway.conf`` of the gateway service or the password printed on your gateway.

This command transmits the certificates to the gateway encrypted with the configuration password. If no error occurs, the gateway is immediately usable.

Open ``/etc/homegear/families/zigbee.conf`` on your Homegear server and add the following lines to the bottom of the file::

    [Gateway]
    id = <commonNameUsed>
    deviceType = homegeargateway
    host = <IP>
    port = 2017
    caFile = /etc/homegear/ca/cacert.pem
    certFile = /etc/homegear/ca/certs/gateway-client.crt
    keyFile = /etc/homegear/ca/private/gateway-client.key

    #use your own 16 bytes hexadecimal key!
    password = 16CFA1797F981EC8651DDD45F8BF0FC6

    responseDelay = 98
    useIdForHostnameVerification = true

Replace ``commonNameUsed`` with the value from above (used for certificate verification) and ``<IP>`` with the IP address of your gateway.

Now restart Homegear and check ``/var/log/homegear/homegear.log`` or ``homegear.err`` for errors.


Additional configuration parameters
***********************************

The following configuration parameters are optional, but you might want to set them::

    panId = "68A3"

This allows setting the PAN ID. If it's not set, a random value will be used. Please use your own value here::

    channelsMask = "1FFE"

This is a channels mask indicating channels to scan when the network is commissioned. Without specifying this setting, the zigbee module will use 0x2000. Network commissioning happens either when a 'reset' command is issued, to reset the communication device, or the first time the communication device is initialized, if a network wasn't already commissioned with it.
Please reset such a device from homegear if it was initialized with some other software.

A lot of devices do not support Zigbee 3. For those, you may use the following setting::

    linkKeyExchange = "no"

Without this setting, only Zigbee 3 devices will be allowed to join. Set this to "no" (notice the double quotes) in order to allow legacy devices to join the network.


Device configuration values
***************************

Devices will have some default values, for attributes and reporting, when paired. Sometimes you might want to have those values changed to your own default values. Those configuration values can be changed by using xml configuration files placed in the zigbee devices configuration directory, ``conf`` subdirectory (default: ``/etc/homegear/devices/26/conf``).

For devices you want homegear to set configuration values, you will need to have xml files with names like ``conf-100bZG9101SAC-HP1.xml``, with values in hexadecimal encoding (use capital letters), with no leading zeros, representing in order: manufacturer code for the device, model identifier and endpoint id. You may find the values with ``config print`` for the peer in CLI.

Here is an example of such file::

	<?xml version="1-0" encoding="utf-8"?>
	<config_values>
		<report cluster="0x8" attr="0x0" type="uint8" minReportingInterval="0x0" maxReportingInterval="300" reportableChange="0x32"/>
	</config_values>

This will change the default reporting values for the attribute 0x0 for cluster 0x8, when pairing.

It is also possible to change the value of an attribute, if it's writeable, when pairing::

	<attr cluster="cluster_id" attr="attr_id" type="attr_type" value="value"/>

Device variables configuration
******************************

When a device is added, it is queried for supported end points, clusters, attributes and commands. Homegear generates variables for the supported attributes and commands, but does that in a generic way that might not be so convenient. For example, one might want to have the configuration values for specific devices easily accessible. There are cases when the generated variables are not sufficient, sometimes devices do not offer enough info about what they support. In some cases, you might know the functionality of some manufacturer specific attributes, for example. Such attributes are not exposed by default by the zigbee module.

For such cases, Homegear provides the possibility of using xml configuration files for devices (default, installed in: ``/etc/homegear/devices/26/``). Currently we provide xml files for several devices, but the list can be extended relatively easy. The format of the xml files is very similar with the format of devices xml configuration files from other Homegear modules.
