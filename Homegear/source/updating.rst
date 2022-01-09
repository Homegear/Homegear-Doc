Updating Homegear
#################

.. highlight:: bash

Updating from Repository
************************

Just run::

	apt-get update
	apt-get upgrade


Manual Update with Package
**************************

First, make a backup of your database in "/var/lib/homegear/db.sql" and of your configuration files.

Download the current version of Homegear, the base library (libhomegear-base), and all installed modules from the `download page <https://www.homegear.eu/downloads.html>`_.

Install the following packages::
	
	dpkg -i libhomegear-base_XXX.deb
	apt-get -f install
	dpkg -i homegear_XXX.deb
	apt-get -f install
	dpkg -i homegear-MODULENAME_XXX.deb
	apt-get -f install

``apt-get -f install`` installs any missing dependencies.

That's it!


Updating from Source
********************

Follow the installation instructions under ":ref:`compiling-homegear`". Before doing anything, you should back up your database in "/var/lib/homegear/db.sql" and your settings in "/etc/homegear".