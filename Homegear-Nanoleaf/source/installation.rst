Installation
############

.. highlight:: bash

Installing from Repository
**************************

If you are using Debian, Raspbian, or Ubuntu, you can install the Nanoleaf module from the repository. Please run the following command as root::

	apt-get install homegear-nanoleaf


Manually Install Debian/Raspbian/Ubuntu Package
***********************************************

Download the Nanoleaf package (homegear-nanoleaf) from the `Homegear nightly download page <https://downloads.homegear.eu/nightlies/>`_ or the `APT repository <https://apt.homegear.eu/>`_. Then install the package as root using dpkg::

	dpkg -i homegear-nanoleaf_*.deb
	apt-get -f install

The command ``apt-get -f install`` installs any missing dependencies.


Compiling from Source
*********************


Compiling Current GitHub Source Using Docker Image
==================================================

See the Homegear documentation.


Manually Compiling Homegear-Nanoleaf
====================================


Prerequisites
-------------

First install all dependencies:
	
	* Homegear
	* Libgpg-error devel
	* GnuTLS devel
	* Libgcrypt devel


Debian/Raspbian/Ubuntu
^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following on Debian, Raspbian, or Ubuntu::

	apt-get install libgpg-error-dev libgnutls28-dev (libgcrypt11-dev or libgcrypt20-dev)


openSUSE Leap
^^^^^^^^^^^^^

On openSUSE Leap, run the following::

	zypper install libtool libgnutls-devel libgpg-error-devel libgcrypt-devel


Compiling
---------

Next, you need to download Homegear-Nanoleaf and extract it::

	wget https://github.com/Homegear/Homegear-Nanoleaf/archive/master.zip
	unzip master.zip
	rm master.zip

Change into the extracted directory and run ``makeRelease.sh`` or ``makeDebug.sh``. You can pass the number of build threads to the script to speed up the compilation process::

	cd Homegear-Nanoleaf-master
	./makeRelease.sh 4


Configuration
-------------


Installing Configuration Files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install the default configuration file::

	cp -R misc/Config\ Directory/nanoleaf.conf /etc/homegear/families

Install the device description files::

	mkdir /etc/homegear/devices/22
	cp -R misc/Device\ Description\ Files/* /etc/homegear/devices/22


First Start
-----------

Now you need to restart Homegear. On Debian, Raspbian, or Ubuntu, you do this with::

	service homegear restart

And watch the log file with the following command to make sure that everything is working properly::

	tail -n 1000 -f /var/log/homegear/homegear.log
