Installation
############

.. highlight:: bash

Installing from Repository
**************************

If you are using Debian, Raspbian, or Ubuntu, you can install the KNX module from the repository. Please run the following command as root::

	apt-get install homegear-knx


Manually Install Debian/Raspbian/Ubuntu Package
***********************************************

Download the KNX package (homegear-knx) from the `Homegear download page <https://www.homegear.eu/index.php/Downloads>`_. Then install the package as root using dpkg::

	dpkg -i homegear-knx_*.deb
	apt-get -f install

The command ``apt-get -f install`` installs any missing dependencies.


Compiling from Source
*********************


Compiling Current GitHub Source Using Docker Image
==================================================

See the Homegear documentation.


Manually Compiling Homegear-KNX
===========================================


Prerequisites
-------------

First install all dependencies:
	
	* Homegear
	* Libgpg-error devel
	* GnuTLS devel
	* Libgcrypt devel
	* Libzip devel


Debian/Raspbian/Ubuntu
^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following on Debian, Raspbian, or Ubuntu::

	apt-get install libgpg-error-dev libgnutls28-dev libzip-dev (libgcrypt11-dev or libgcrypt20-dev)


openSUSE Leap
^^^^^^^^^^^^^

On openSUSE Leap, run the following::

	zypper install libtool libgnutls-devel libgpg-error-devel libgcrypt-devel libzip-devel


Compiling
---------

Next, you need to download Homegear-KNX and extract it::

	wget https://github.com/Homegear/HomeGear-KNX/archive/master.zip
	unzip master.zip
	rm master.zip

Change into the extracted directory and run ``makeRelease.sh`` or ``makeDebug.sh``. You can pass the number of build threads to the script to speed up the compilation process::

	cd Homegear-KNX-master
	./makeRelease.sh 4


Configuration
-------------


Installing Configuration Files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install the default configuration file::

	cp -R misc/Config\ Directory/knx.conf /etc/homegear/families

Create the device description directory::

	mkdir /etc/homegear/devices/14

.. note:: In contrast to other modules the KNX module does not have any device description XML files. The XML files are generated by placing an ETS project file in the device description directory and then executing the CLI command ``search`` or the RPC command ``searchDevices``.


Communication Modules
^^^^^^^^^^^^^^^^^^^^^

See :ref:`communication-modules` for instructions on how to configure communication modules.


First Start
-----------

Now you need to restart Homegear. On Debian, Raspbian, or Ubuntu, you do this with::

	service homegear restart

And watch the log file with the following command to make sure that everything is working properly::

	tail -n 1000 -f /var/log/homegear/homegear.log