Installation
############

.. highlight:: bash

System Requirements
*******************

See :ref:`hardware-and-software-requirements`.


Installing from Repository
**************************

If you are using Debian, Raspbian, or Ubuntu, you can install Homegear from the repository. To do so, please follow the instructions for your system.


Debian
======

Packages are provided for Debian 8 (Jessie) and Debian 9 (Stretch). Supported architectures are i386, amd64, armel, and armhf.

.. warning:: Don't use the Debian repositories for Raspbian. Use the Raspbian repository instead.


Debian 8 (Jessie)
-----------------

Please run the following commands as root::

	apt install apt-transport-https
	wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
	echo 'deb https://apt.homegear.eu/Debian/ jessie/' >> /etc/apt/sources.list.d/homegear.list
	apt update
	apt install homegear homegear-nodes-core homegear-management

After installing Homegear, you can install any family modules you like. To install all available family modules, run the following::

	apt install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-ipcam homegear-kodi homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-nanoleaf homegear-ccu2 homegear-mbus homegear-influxdb


Debian 9 (Stretch)
------------------

Please run the following commands as root::

	apt install apt-transport-https
	wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
	echo 'deb https://apt.homegear.eu/Debian/ stretch/' >> /etc/apt/sources.list.d/homegear.list
	apt update
	apt install homegear homegear-nodes-core homegear-management

After installing Homegear, you can install any family modules you like. To install all available family modules, run the following::

	apt install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-ipcam homegear-kodi homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-nanoleaf homegear-ccu2 homegear-mbus homegear-influxdb


Raspbian
========

Packages are provided for Raspbian 8 (Jessie) and Raspbian 9 (Stretch).


Raspbian 8 (Jessie)
-------------------

Please run the following commands as root::

	apt install apt-transport-https
	wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
	echo 'deb https://apt.homegear.eu/Raspbian/ jessie/' >> /etc/apt/sources.list.d/homegear.list
	apt update
	apt install homegear homegear-nodes-core homegear-management

After installing Homegear, you can install any family modules you like. To install all available family modules, run the following::

	apt install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-ipcam homegear-kodi homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-nanoleaf homegear-ccu2 homegear-mbus homegear-influxdb


Raspbian 9 (Stretch)
--------------------

Please run the following commands as root::

	apt install apt-transport-https
	wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
	echo 'deb https://apt.homegear.eu/Raspbian/ stretch/' >> /etc/apt/sources.list.d/homegear.list
	apt update
	apt install homegear homegear-nodes-core homegear-management

After installing Homegear, you can install any family modules you like. To install all available family modules, run the following::

	apt install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-ipcam homegear-kodi homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-nanoleaf homegear-ccu2 homegear-mbus homegear-influxdb


Ubuntu
======

Packages are provided for Ubuntu 14.04 (Trusty Tahr) and Ubuntu 15.10 (Wily Werewolf).


Ubuntu 14.04 (Trusty Tahr)
--------------------------

Please run the following commands as root::

	apt install apt-transport-https
	wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
	echo 'deb https://apt.homegear.eu/Ubuntu/ trusty/' >> /etc/apt/sources.list.d/homegear.list
	apt update
	apt install homegear homegear-nodes-core homegear-management

After installing Homegear, you can install any family modules you like. To install all available family modules, run the following::

	apt install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-ipcam homegear-kodi homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-nanoleaf homegear-ccu2 homegear-mbus homegear-influxdb


Ubuntu 16.04 (Xenial Xerus)
----------------------------

Please run the following commands as root::

	apt install apt-transport-https
	wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
	echo 'deb https://apt.homegear.eu/Ubuntu/ xenial/' >> /etc/apt/sources.list.d/homegear.list
	apt update
	apt install homegear homegear-nodes-core homegear-management

After installing Homegear, you can install any family modules you like. To install all available family modules, run the following::

	​apt install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-ipcam homegear-kodi homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-nanoleaf homegear-ccu2 homegear-mbus homegear-influxdb


Ubuntu 18.04 (Bionic Beaver)
----------------------------

Please run the following commands as root::

	apt install apt-transport-https
	wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
	echo 'deb https://apt.homegear.eu/Ubuntu/ bionic/' >> /etc/apt/sources.list.d/homegear.list
	apt update
	apt install homegear homegear-nodes-core homegear-management

After installing Homegear, you can install any family modules you like. To install all available family modules, run the following::

	​apt install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos homegear-ipcam homegear-kodi homegear-beckhoff homegear-knx homegear-enocean homegear-intertechno homegear-nanoleaf homegear-ccu2 homegear-mbus homegear-influxdb


Arch Linux
==========

Packages for Arch Linux are provided in the `Arch User Repository (AUR) <https://aur.archlinux.org>`_. Use wget or your preferred `AUR helper <https://wiki.archlinux.org/index.php/AUR_helpers>`_ for downloading these base packages:

* homegear-git
* php7-homegear
* libhomegear-base-git
* termcap

Download also the packages for the family modules you want to use:

* homegear-homematicbidcos-git
* homegear-enocean-git

Arch Linux for Raspberry Pi
---------------------------

Preparing the PKGBUILD-files
    Many of the PKGBUILD-files contain an explicit declaration of the possile architectures like ``arch=('i686' 'x86_64')``. However, the above listed packages are working also at the ARM architecture of a Raspberry Pi. Edit the related PKBUILD-files and insert ``'armv6h'`` to the list of architectures.

**Compile the sources**

Your Raspberry should have at least 512 MB of RAM for compiling the sources. Use the command ``makepkg`` to build the packages.

**Install the packages**

Install the packages the common way with the command ``pacman -U`` . The packages may also be installed on a Raspberry Pi of the first generation with only 256MB of RAM.

**Configure the System**

You have to create a homegear user and some directories. Just run the following commands::

   useradd –system -U –no-create-home homegear
   mkdir /var/log/homegear
   chmod 750 /var/log/homegear
   chown homegear:homegear /var/log/homegear
   chmod 750 /var/lib/homegear
   chown homegear:homegear /var/lib/homegear

uncomment the following line in /etc/php/php.ini::

    extension=xmlrpc.so

Create keys for SSL/TLS encryption::

    openssl genrsa -out /etc/homegear/homegear.key 2048
    ​openssl req -batch -new -key /etc/homegear/homegear.key -out /etc/homegear/homegear.csr
    ​openssl x509 -req -in /etc/homegear/homegear.csr -signkey /etc/homegear/homegear.key -out /etc/homegear/homegear.crt
    ​rm /etc/homegear/homegear.csr
    ​chown homegear:homegear /etc/homegear/homegear.key
    ​chmod 400 /etc/homegear/homegear.key
    ​openssl dhparam -check -text -5 1024 -out /etc/homegear/dh1024.pem
    ​chown homegear:homegear /etc/homegear/dh1024.pem
    ​chmod 400 /etc/homegear/dh1024.pem

Insert the following lines in /etc/homegear/main.conf in the section [Service]::

    runAsUser = homegear
    runAsGroup = homegear

**Create a suitable systemd service file**

copy the default service file with::

    cp /usr/lib/systemd/system/homegear.service /etc/systemd/system/myhomegear.service

and insert the following content in myhomegear.service::

    User=homegear
    Group=homegear
    RuntimeDirectory=homegear

With these lines, the homegear server will run by the user homegear and they provide a directory under /var/run owned and writable by the user homegear.

**Configure the communication hardware**

Follow the instructions described here: `<https://doc.homegear.eu/data/homegear-homematicbidcos/configuration.html#config-coc>`_

If you are planning to use a COC device, some further configurations are necessary in Arch Linux. The user homegear has to be member of the group uucp to use /dev/ttyAMA0::

    gpasswd -a homegear uucp

Install the package wiringpi-git from AUR to provide user access to the GPIO hardware. Then add the following lines to the [Service] section in /etc/systemd/system/myhomegear.service::

    ExecStartPre=/usr/bin/gpio export 17 out
    ExecStartPre=/usr/bin/gpio export 18 out
    ExecStop=/usr/bin/gpio unexport 17
    ExecStop=/usr/bin/gpio unexport 18

The full /etc/systemd/system/myhomegear.service file may look like::

    [Unit]
    Description=Homegear server
    After=network.target

    [Service]
    Type=simple
    User=homegear
    Group=homegear
    UMask=002
    LimitRTPRIO=100
    ExecStartPre=/usr/bin/gpio export 17 out
    ExecStartPre=/usr/bin/gpio export 18 out
    RuntimeDirectory=homegear
    ExecStart=/usr/bin/homegear
    ExecStop=/usr/bin/gpio unexport 17
    ExecStop=/usr/bin/gpio unexport 18

    [Install]
    WantedBy=multi-user.target

**Start the server**

Run the following commands to start and enable the homegear server with systemd::

    systemctl daemon-reload
    systemctl start myhomegear
    systemctl enable myhomegear



Manually Install Debian/Raspbian/Ubuntu Package
***********************************************

Download the proper packages from the `Homegear nightly download page <https://downloads.homegear.eu/nightlies/>`_ or the `APT repository <https://apt.homegear.eu/>`_. At the very least, you need the packages ``libhomegear-base`` and ``homegear``. Additionally, you should download all family module packages you want to use. Then, as root, install the packages using dpkg::

	dpkg -i libhomegear-base_XXX.deb
	​apt-get -f install
	​dpkg -i homegear_XXX.deb
	​apt-get -f install
	dpkg -i homegear-nodes-core_XXX.deb
	​apt-get -f install
	​dpkg -i homegear-MODULENAME_XXX.deb
	​apt-get -f install

``apt-get -f install`` installs any missing dependencies.


Raspbian Image
**************

The easiest way to use Homegear on a Raspberry Pi is to `download the Raspberry Pi image <https://www.homegear.eu/downloads.html>`_ and write it to an SD card.

Follow the instructions on `elinux.org <http://elinux.org/RPi_Easy_SD_Card_Setup#Flashing_the_SD_Card_using_Windows>`_ to transfer the image to your SD card (for Windows, Mac, and GNU/Linux).

.. note:: The username is ``pi``, and the password is ``raspberry``.

Because SSH is enabled on port 22, you can use an SSH client (such as PuTTY) to log in, and you don't need to connect a display or a keyboard. You can try logging in using the hostname ``homegearpi``. Alternatively, you would need to look up the IP address of your DHCP server (or router). The first time you log in, the Raspberry Pi configuration tool will start.


Compiling from Source
*********************


Compiling Current GitHub Source Using Docker Image
==================================================

The easiest way to compile Homegear from the source is by using Docker. Docker images are provided for Debian 8 (Jessie; amd64, i386, armhf, arm64, armel), Debian 9 (Stretch; amd64, i386, armhf, arm64), Raspbian Jessie, Raspbian Stretch, Ubuntu 14.04 (Trusty Tahr; amd64, i386, armhf, arm64), and Ubuntu 15.10 (Wily Werewolf; amd64, i386, armhf, arm64). Start the Docker image by running the following command::

	docker run -it -e HOMEGEARBUILD_SHELL=1 homegear/build:TAG

Replace "TAG" with one of the tags from `the repository <https://hub.docker.com/r/homegear/build/tags/>`_ (such as debian-jessie-amd64). You need to set the environment variable to avoid being asked for information about the server to which you want to upload the created packages. To speed up compilation, you can also set ``HOMEGEARBUILD_THREADS`` to the number of CPU cores in your system.

In the container, execute::

	/build/CreateDebianPackageNightly.sh

Once that is finished, you can find the created Debian packages in the directory ``/build``.

.. _compiling-homegear:

Manually Compiling Homegear
===========================

.. _compiling-php:

Compiling PHP
-------------


Debian / Ubuntu / Raspbian
^^^^^^^^^^^^^^^^^^^^^^^^^^

Homegear is available for all systems as a Debian package. You can get the required PHP library and header files by installing "php7-homegear-dev" using apt::

	apt-get install php7-homegear-dev


Prerequisites
^^^^^^^^^^^^^

For all other systems, you need to compile PHP 7 from the source. But first of all, you need to install the prerequisites.


openSUSE Leap
"""""""""""""

Execute::

	zypper install autoconf gcc gcc-c++ libxml2-devel libopenssl-devel enchant-devel gmp-devel libmcrypt-devel libedit-devel


Compiling
^^^^^^^^^

.. warning:: Homegear requires at least PHP 7.2 as ZTS is broken in PHP 7.0 and 7.1.

Download the PHP source code from the `PHP download page <http://php.net/downloads.php>`_. Then extract the package::

	tar -zxf php-7.X.X.tar.gz

or::

	tar -jxf php-7.X.X.tar.bz2

Switch to the subdirectory "ext" within the extracted directory::

	cd php-7.X.X/ext

Clone the current version of pthreads from `GitHub <https://github.com/krakjoe/pthreads/releases>`_::

	git clone https://github.com/krakjoe/pthreads.git

Switch to the parent directory::

	cd ..

Execute autoconf::

	autoconf

Execute the configure script. The line before the script is also necessary; they get the target system (e. g. ``x86_64-linux-gnu``)::

	target="$(gcc -v 2>&1)" && strpos="${target%%Target:*}" && strpos=${#strpos} && target=${target:strpos} && target=$(echo $target | cut -d ":" -f 2 | cut -d " " -f 2)
	​./configure  --prefix /usr/share/homegear/php --enable-embed=static --with-config-file-path=/etc/homegear --with-config-file-scan-dir=/etc/homegear/php.conf.d --includedir=/usr/include/php7-homegear --libdir=/usr/share/homegear/php --libexecdir=${prefix}/lib --datadir=${prefix}/share --program-suffix=-homegear --sysconfdir=/etc/homegear --localstatedir=/var --mandir=${prefix}/man --disable-debug --disable-rpath --with-pic --with-layout=GNU --enable-bcmath --enable-calendar --enable-ctype --enable-dba --without-gdbm --without-qdbm --enable-inifile --enable-flatfile --enable-dom --with-enchant=/usr --enable-exif --with-gettext=/usr --with-gmp=/usr/include/$target --enable-fileinfo --enable-filter --enable-ftp --enable-hash --enable-json --enable-pdo --enable-mbregex --enable-mbregex-backtrack --enable-mbstring --disable-opcache --enable-phar --enable-posix --with-mysqli=mysqlnd --with-zlib-dir=/usr --with-openssl --with-libedit=/usr --enable-libxml --enable-session --enable-simplexml --enable-pthreads --with-xmlrpc --enable-soap --enable-sockets --enable-tokenizer --enable-xml --enable-xmlreader --enable-xmlwriter --with-mhash=yes --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-zip --disable-cli --disable-cgi --enable-pcntl --enable-maintainer-zts

If dependencies are missing, install them and run the configure script again until it finishes successfully. You can also remove dependencies, if they are not needed. When this is done, run::

	make && make install
	cp /usr/share/homegear/php/lib/libphp7.a /usr/lib/libphp7-homegear.a


Compiling Homegear
------------------


Prerequisites
^^^^^^^^^^^^^

First, install all dependencies:

* Libtool
* Automake
* PHP 7 devel and static library (see :ref:`compiling-php`)
* SQLite 3 devel
* Readline 6 devel
* Libgpg-error devel
* GnuTLS devel
* Libgcrypt devel
* Libxslt devel (needed by PHP)
* OpenSSL devel (needed by PHP)
* Libmysqlclient devel (needed by PHP)
* Unzip (for extracting the source code)


Debian / Raspbian / Ubuntu
""""""""""""""""""""""""""

Run the following command on Debian, Raspbian, or Ubuntu::

	apt-get install libsqlite3-dev libreadline6-dev libgpg-error-dev libgnutls28-dev libxslt-dev libssl-dev libmysqlclient-dev unzip libtool automake (libgcrypt11-dev or libgcrypt20-dev)


openSUSE Leap
"""""""""""""

On openSUSE Leap, run::

	zypper install libtool libgnutls-devel libgpg-error-devel sqlite3-devel libgcrypt-devel libxslt-devel


Compiling
^^^^^^^^^

Then download Homegear's base library and extract it::

	wget https://github.com/Homegear/libhomegear-base/archive/master.zip
	​unzip master.zip
	​rm master.zip

Switch to the extracted directory and run ``makeRelease.sh`` or ``makeDebug.sh``. You can pass the number of build threads to the script to speed up compilation::

	cd libhomegear-base-master
	./makeRelease.sh 4

Then do the same for Homegear's node library::

	wget https://github.com/Homegear/libhomegear-node/archive/master.zip
	​unzip master.zip
	​rm master.zip
	​cd libhomegear-node-master
	​./makeRelease.sh 4

For Homegear::

	wget https://github.com/Homegear/Homegear/archive/master.zip
	​unzip master.zip
	​rm master.zip
	​cd Homegear-master
	​./makeRelease.sh 4

And the core nodes::

	wget https://github.com/Homegear/homegear-nodes-core/archive/master.zip
	​unzip master.zip
	​rm master.zip
	​cd homegear-nodes-core-master
	​./makeRelease.sh 4

You can also compile the optional Homegear Management service::

	wget https://github.com/Homegear/homegear-management/archive/master.zip
	​unzip master.zip
	​rm master.zip
	​cd homegear-management-master
	​./makeRelease.sh 4

Repeat these steps for all family modules you want to compile.


Configuration
^^^^^^^^^^^^^

First, add a user named homegear::

	useradd --system -U --no-create-home homegear

Copy the default configuration files from the directory containing the files of Homegear's main project::

	cd ../Homegear-master
	cp -R misc/Config\ Directory /etc/homegear

Also copy the Homegear Management configuration files (if Homegear Management was compiled)::

    cd ../homegear-management-master
    cp -R misc/Config\ Directory/* /etc/homegear

Now setup all necessary directories::

	mkdir /var/log/homegear
	​chmod 750 /var/log/homegear
	​chown homegear:homegear /var/log/homegear
	
	mkdir /var/log/homegear-management
	​chmod 750 /var/log/homegear-management
	​chown homegear:homegear /var/log/homegear-management
	
	mkdir /var/lib/homegear
	​chmod 750 /var/lib/homegear
	​chown homegear:homegear /var/lib/homegear

Finally, create the certificates required for SSL/TLS encryption::

	openssl genrsa -out /etc/homegear/homegear.key 2048
	​openssl req -batch -new -key /etc/homegear/homegear.key -out /etc/homegear/homegear.csr
	​openssl x509 -req -in /etc/homegear/homegear.csr -signkey /etc/homegear/homegear.key -out /etc/homegear/homegear.crt
	​rm /etc/homegear/homegear.csr
	​chown homegear:homegear /etc/homegear/homegear.key
	​chmod 400 /etc/homegear/homegear.key
	​openssl dhparam -check -text -5 1024 -out /etc/homegear/dh1024.pem
	​chown homegear:homegear /etc/homegear/dh1024.pem
	​chmod 400 /etc/homegear/dh1024.pem


First Start
^^^^^^^^^^^

Now try to start Homegear with ::

	homegear -u homegear -g homegear -d

and watch the log file using the following command to see if everything is working correctly::

	tail -n 1000 -f /var/log/homegear/homegear.log


Clients Without SSL Support
***************************

If you want to connect a client that doesn't support SSL, we strongly recommend setting up an SSH tunnel or using a VPN (such as OpenVPN) to encrypt your connection.


Create Homegear's Certificate Authority
***************************************

If you want to use Homegear Gateways or the Homegear Gateway service, you need to create a certificate authority to create gateway and client certificates. The easiest way to do that is by using Homegear's Managament service. Note that creating the CA this way changes your `/etc/ssl/openssl.cnf`::

    homegear -e rc 'print_v($hg->managementCreateCa());'

This creates a CA in ``/etc/homegear/ca`` in background. To check if the command has finished, execute::

    homegear -e rc 'print_v($hg->managementGetCommandStatus());'

This returns the command output and the exit code. The command has finished if the exit code is other than ``256``. On success the exit code is ``0``.

Next create the client certificate to login into gateways::

    homegear -e rc 'print_v($hg->managementCreateCert("gateway-client"));'

Again this command runs in background and you can check if the command has finished with::

    homegear -e rc 'print_v($hg->managementGetCommandStatus());'


Install a User Interface
************************

Homegear does not have a web user interface yet. Until it does, you can use:

* `HomeMatic Manager <https://github.com/hobbyquaker/homematic-manager>`_
* `HomeMatic Configuration Tool coming with the BidCoS Service (in German only)  <http://www.eq-3.de/Downloads/Software/Konfigurationsadapter/Konfigurationsadapter_LAN/HM-CFG-LAN_Usersoftware_V1_520_eQ-3_151207.zip>`_
* `HomegearLib.NET Test Application <https://github.com/Homegear/HomegearLib.NET/releases>`_
