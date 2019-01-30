Configuration
#############

.. highlight:: bash

.. note:: If you used the communication module with some other software and added devices to it, you might need first to unpair the devices with that software and maybe even reset the communication module. Alternatively you may reset the devices manually and also reset the communication device from homegear. Homegear might be able to recognize and create the devices already added, at startup, but if they are added securely with a different key, that will not work.

z-wave.conf
***********

The configuration file for the Z-Wave module, ``z-wave.conf``, can be found in Homegear's family configuration directory (default: ``/etc/homegear/families``). In this file, you can configure the communication modules used to communicate with Z-Wave devices.

Communication Modules
*********************

Overview
========

The Z-Wave module supports all communication modules using the USB interface.

On Raspberry Pi it also supports communication modules that use the gpio interface.

The Z-Wave module can also connect to a remote usb stick or gpio communication module using the Homegear Gateway service:

    * :ref:`Homegear Gateway <config-homegear-gateway>`


.. serial:

Serial
======

To tell Homegear to use the usb module, insert the following lines into ``z-wave.conf``::

	[Serial]

	id = Serial1
	deviceType = serial

	#use your own 16 bytes hexadecimal key!
	password = 16CFA1797F981EC8651DDD45F8BF0FC6

	device = /dev/serial/by-id/usb-0658_0200-if00


Of course, you can use multiple communication modules with Homegear. We recommend to use the gateway (see below) in such a case, but you could use more than one local device. The downsize is that you have to pull out the usb devices except one when you add a z-wave node, because pairing activates all interfaces.

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

First you need to create certificates for the Gateway service. We don't want an insecure gateway so there is no possibility to use it without creating them. If not done already, start by following the instructions `to create a certificate authority in the Homegear manual <https://doc.homegear.eu/data/homegear/installation.html#create-homegear-s-certificate-authority>`_.

First create the gateway certificates using Homegear Management::

    homegear -e rc 'print_v($hg->managementCreateCert("my-gateway"));'

Replace ``my-gateway`` with an arbitrary name (it doesn't need to be the hostname of the gateway). The name will be used to set the field ``COMMON NAME`` of the certificate. It has to be the same as set to the setting ``id`` in ``z-wave.conf`` (see below).

The output of the command looks similar to::

    (Struct length=5)
    {
      [caPath]
      {
        (String) /etc/homegear/ca/cacert.pem
        {
          [certPath]
          {
            (String) /etc/homegear/ca/certs/z-wave-gateway-01.crt
          }
          [commonNameUsed]
          {
            (String) z-wave-gateway-01
          }
          [filenamePrefix]
          {
            (String) z-wave-gateway-01
          }
          [keyPath]
          {
            (String) /etc/homegear/ca/private/z-wave-gateway-01.key
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


Open ``/etc/homegear/gateway.conf`` and set the settings for your communication module, e. g. for an USB stick on device ``/dev/serial/by-id/usb-0658_0200-if00``::

    family = z-wave
    device = /dev/serial/by-id/usb-0658_0200-if00

Note the ``configurationPassword``, we need below.

Restart the gateway service.

    service homegear-gateway restart


Check ``/var/log/homegear-gateway/homegear-gateway.log`` for errors. If everything is working, the logfile should say ``Startup complete`` and print a warning that the gateway is unconfigured.

.. note:: To reset a gateway (make it "unconfigured"), delete the files ``<dataPath>/ca.crt``, ``<dataPath>/gateway.crt`` and ``<dataPath>/gateway.key``. ``dataPath`` is configured in ``/etc/homegear/gateway.conf``.


Homegear
--------

To configure a gateway, execute::

    homegear -e rc '$hg->configureGateway("<IP>", 2018, file_get_contents("/etc/homegear/ca/cacert.pem"), file_get_contents("/etc/homegear/ca/certs/<your-cert>.crt"), file_get_contents("/etc/homegear/ca/private/<your-cert>.key"), "<your-configuration-password>");'

Replace ``<your-cert>`` with the value of ``commonNameUsed`` from above, ``<IP>`` with the IP address of your gateway and ``<your-configuration-password>`` with ``configurationPassword`` from the ``gateway.conf`` of the gateway service or the password printed on your gateway.

This command transmits the certificates to the gateway encrypted with the configuration password. If no error occurs, the gateway is immediately usable.

Open ``/etc/homegear/families/z-wave.conf`` on your Homegear server and add the following lines to the bottom of the file::

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


Device configuration values
***************************

Devices supporting the configuration class will have some default values when paired. Sometimes you might want to have those values changed to your own default values. Those configuration values can be changed by using xml configuration files placed in the z-wave devices configuration directory, ``conf`` subdirectory (default: ``/etc/homegear/devices/17/conf``).

For devices you want homegear to set configuration values, you will need to have xml files with names like ``conf-86-2-64.xml``, with values in hexadecimal encoding (use capital letters), with no leading zeros, representing in order: manufacturer id for the device, product type and product id. You may find the values with ``config print`` for the peer in CLI.

Here is an example of such file::

	<?xml version="1.0" encoding="utf-8"?>
	<config_values>
	  <config_value index="3">60</config_value>
	  <config_value index="5">2</config_value>
	</config_values>
