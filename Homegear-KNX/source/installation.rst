Installation
############

.. highlight:: bash

Installing from Repository
**************************

If you are using Debian, Raspbian, or Ubuntu, you can install the KNX module from the repository. Please run the following command as root::

	apt-get install homegear-knx


Manually Install Debian/Raspbian/Ubuntu Package
***********************************************

Download the KNX package (homegear-knx) from the `Homegear download page <https://homegear.eu/downloads/nightlies/>`_. Then install the package as root using dpkg::

	dpkg -i homegear-knx_*.deb
	apt-get -f install

The command ``apt-get -f install`` installs any missing dependencies.
