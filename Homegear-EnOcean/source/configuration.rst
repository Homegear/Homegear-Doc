Configuration
#############

.. highlight:: bash

enocean.conf
************

The configuration file for the EnOcean module, ``enocean.conf``, can be found in Homegear's family configuration directory (default: /etc/homegear/families). In this file, you can configure the communication modules used to communicate with EnOcean devices.


Communication Modules
*********************

Overview
========

The EnOcean module supports all communication modules using the TCM310:

	* :ref:`EnOcean USB 300 <config-tcm310>`
	* :ref:`EnOcean Pi Wireless Module <config-tcm310>`

The TCM310 can also be connected to additional SBCs using the Homegear Gateway service:

    * :ref:`Homegear Gateway <config-homegear-gateway>`

We recommend using a module with a good antenna as this greatly increases the range. The chip antenna on the USB 300 is definitely not recommended.


.. _config-tcm310:

TCM310
======

To tell Homegear to use the module, insert the following lines into ``enocean.conf``::

	[TCM310]
    id = TCM310
    deviceType = tcm310
    device = /dev/ttyUSB0

Of course, you can use multiple communication modules with Homegear.

For USB devices this is all. In case you are using a UART device like the EnOcean Pi, additionally follow these steps:


Free Up Serial Line and Enable UART
-----------------------------------

All Raspberry Pis
^^^^^^^^^^^^^^^^^

ttyAMA0 or serial0 might be used by the serial console. To free it up do the following.

Remove any references to ttyAMA0 and serial0 from /etc/inittab and /boot/cmdline.txt.

Our /boot/cmdline.txt looks like this::

    dwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait


Raspberry Pi 3
^^^^^^^^^^^^^^

On the Raspberry Pi 3 /dev/ttyAMA0 is used by the Wifi and Bluetooth module. There is a "mini UART" available on /dev/ttyS0 by default. It is better though, to use the hardware UART and switch the Wifi/Bluetooth module to mini UART. To do that, add this line at the end of ``/boot/config.txt``::

    dtoverlay=pi3-miniuart-bt

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

Replace ``my-gateway`` with an arbitrary name (it doesn't need to be the hostname of the gateway). The name will be used to set the field ``COMMON NAME`` of the certificate. It has to be the same as set to the setting ``id`` in ``enocean.conf`` (see below).

The output of the command looks similar to::

    (Struct length=5)
    {
      [caPath]
      {
        (String) /etc/homegear/ca/cacert.pem
        {
          [certPath]
          {
            (String) /etc/homegear/ca/certs/enocean-gateway-01.crt
          }
          [commonNameUsed]
          {
            (String) enocean-gateway-01
          }
          [filenamePrefix]
          {
            (String) enocean-gateway-01
          }
          [keyPath]
          {
            (String) /etc/homegear/ca/private/enocean-gateway-01.key
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


Open ``/etc/homegear/gateway.conf`` and set the settings for your communication module, e. g. for an USB 300 stick on device ``ttyUSB0``::

    family = EnOcean
    device = /dev/ttyUSB0

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

Open ``/etc/homegear/families/enocean.conf`` on your Homegear server and add the following lines to the bottom of the file::

    [Homegear Gateway]
    id = <commonNameUsed>
    deviceType = homegeargateway
    host = <IP>
    port = 2017
    caFile = /etc/homegear/ca/cacert.pem
    certFile = /etc/homegear/ca/certs/gateway-client.crt
    keyFile = /etc/homegear/ca/private/gateway-client.key
    responseDelay = 98
    useIdForHostnameVerification = true

Replace ``commonNameUsed`` with the value from above (used for certificate verification) and ``<IP>`` with the IP address of your gateway.

Now restart Homegear and check ``/var/log/homegear/homegear.log`` or ``homegear.err`` for errors.