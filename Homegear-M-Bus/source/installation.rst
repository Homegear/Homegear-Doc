Installation
############

.. highlight:: bash

Installing from Repository
**************************

If you are using Debian, Raspbian, or Ubuntu, you can install the M-Bus module from the repository. Please run the following command as root::

	apt-get install homegear-mbus


Manually Install Debian/Raspbian/Ubuntu Package
***********************************************

Download the M-Bus package (homegear-mbus) from the `Homegear nightly download page <https://downloads.homegear.eu/nightlies/>`_ or the `APT repository <https://apt.homegear.eu/>`_. Then install the package as root using dpkg::

	dpkg -i homegear-mbus_*.deb
	apt-get -f install

The command ``apt-get -f install`` installs any missing dependencies.


First Start
***********

After installing the module, you need to restart Homegear. On Debian, Raspbian, or Ubuntu, you do this with::

	service homegear restart

Watch the log file with the following command to make sure that everything is working properly::

	tail -n 1000 -f /var/log/homegear/homegear.log
