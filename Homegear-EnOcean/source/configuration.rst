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

	* EnOcean USB 300
	* EnOcean Pi Wireless Module

We recommend using a module with a good antenna as this greatly increases the range. The chip antenna on the USB 300 is definitely not recommended.


TCM310
======

To tell Homegear to use the module, insert the following lines into ``enocean.conf``::

	[TCM310]
    id = TCM310
    deviceType = usb300
    device = /dev/ttyS0


Of course, you can use multiple communication modules with Homegear.


Homegear Gateway
================

First you need to create certificates for the Gateway service and for authentication. We don't want an insecure gateway so there is no possibility to use it without creating them. Here are the steps to create a certificate authority in Debian or Ubuntu and to create the necessary certificates:

First make sure the date and time are set correctly. Then edit the OpenSSL configuration file ``/usr/lib/ssl/openssl.cnf`` and change ::

    dir             = ./demoCA


in section ``[ CA_default ]`` to ::

    dir             = /root/ca


Create the directory ``/root/ca``::

    mkdir /root/ca


and create some necessary subfolders::

    cd /root/ca
    mkdir newcerts certs crl private requests


Create the files ``index.txt`` and ``serial``::

    touch index.txt
    echo "1000" > serial


Now generate the CA's private key::

    openssl genrsa -aes256 -out private/cakey.pem 4096


Create the CA certificate. Set common name to e. g. ``Homegear CA``::

    openssl req -new -x509 -key /root/ca/private/cakey.pem -out cacert.pem -days 10958 -set_serial 0


The certificate is saved to ``/root/ca/cacert.pem`` and is valid for 30 years.

Now we can create and sign certificates. First lets create the certificates for the gateway. Enter the correct hostname for common name. This name is verified when Homegear connects to the gateway, so the gateway must be reachable under that name from your Homegear installation. If the hostname can't be resolved using DNS, you can create an entry for the gateway in ``/etc/hosts`` (e. g. ``192.168.178.11   homegeargateway``) on your system running Homegear. Don't set the "challenge password". ::

    openssl genrsa -aes256 -out private/homegeargateway.enc.key 2048
    openssl req -new -key private/homegeargateway.enc.key -out newcert.csr
    openssl ca -in newcert.csr -out certs/homegeargateway.crt


.. important:: You need to set the correct hostname for ``COMMON NAME`` and also use this hostname to connect to the gateway (not the IP)!


Next lets create the client certificate your Homegear system uses to login to the gateway. Again don't set the "challenge password". ::

    openssl genrsa -aes256 -out private/homegearclient.enc.key 2048
    openssl req -new -key private/homegearclient.enc.key -out newcert.csr
    openssl ca -in newcert.csr -out certs/homegearclient.crt


.. warning:: The common name needs to be unique. When you get the error ``TXT_DB error number 2`` open the file ``index.txt`` and remove the line with the common name of the certificate you just tried to create. Then create the certificate again.


Now all certificates are created.


Homegear Gateway
----------------

Setup the gateway computer with Debian, Raspbian or Ubuntu first and connect an EnOcean serial module or USB stick.

Then install Homegear Gateway::

    apt install apt-transport-https
    curl https://apt.homegear.eu/Release.key | sudo apt-key add -
    echo 'deb https://apt.homegear.eu/Debian/ stretch/' >> /etc/apt/sources.list.d/homegear.list
    apt update
    apt install homegear-gateway


Copy the certificates ``cacert.pem``, ``homegeargateway.enc.key`` and ``homegeargateway.crt`` to ``/etc/homegear/`` on the gateway system. Decrypt the private key and set appropriate permissions::

    cd /etc/homegear
    openssl rsa -in homegeargateway.enc.key -out homegeargateway.key
    chmod 400 homegeargateway.key
    chown homegear:homegear homegeargateway.key


Create the Diffie-Hellman parameter file::

    openssl dhparam -check -text -5 -out dh1024.pem 1024


Open ``/etc/homegear/gateway.conf`` and set the following settings::

    caFile = /etc/homegear/cacert.pem
    certPath = /etc/homegear/homegeargateway.crt
    keyPath = /etc/homegear/homegeargateway.key
    dhPath = /etc/homegear/dh1024.pem

    family = EnOcean
    device = /dev/ttyS0


Set ``device`` to the serial device the EnOcean module is connected to. Now restart the gateway service::

    service homegear-gateway restart


Check ``/var/log/homegear-gateway/homegear-gateway.log`` for errors. If everything is working, the logfile should say ``Startup complete``.


Homegear
--------

Copy the certificates ``cacert.pem``, ``homegearclient.enc.key`` and ``homegearclient.crt`` to ``/etc/homegear/`` on the gateway system. Decrypt the private key and set appropriate permissions::

    cd /etc/homegear
    openssl rsa -in homegearclient.enc.key -out homegearclient.key
    chmod 400 homegearclient.key
    chown homegear:homegear homegearclient.key


Open ``/etc/homegear/families/enocean.conf`` and add the following lines to the bottom of the file::

    [Homegear Gateway]
    id = My-Gateway
    deviceType = homegeargateway
    # The host name of the Homegear gateway
    host = homegeargateway
    port = 2017
    caFile = /etc/homegear/cacert.pem
    certFile = /etc/homegear/homegearclient.crt
    keyFile = /etc/homegear/homegearclient.key


Now restart Homegear and check ``/var/log/homegear/homegear.log`` or ``homegear.err`` for errors.