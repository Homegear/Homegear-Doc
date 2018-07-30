Installation
############

.. highlight:: bash

Installing from Repository
**************************

If you are using Debian, Raspbian, or Ubuntu, you can install the Z-Wave module from the repository. Please run the following command as root::

	apt-get install homegear-zwave


Manually Install Debian/Raspbian/Ubuntu Package
***********************************************

Download the Z-Wave package (homegear-zwave) from the `Homegear nightly download page <https://downloads.homegear.eu/nightlies/>`_ or the `APT repository <https://apt.homegear.eu/>`_. Then install the package as root using dpkg::

	dpkg -i homegear-zwave_*.deb
	apt-get -f install

The command ``apt-get -f install`` installs any missing dependencies.
