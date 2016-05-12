Updating Homegear
#################

.. highlight:: bash

Updating From Repository
************************

Just run::

	apt-get update
	apt-get upgrade


Manual Update with Package
**************************

First make a backup of your database in "/var/lib/homegear/db.sql" and your configuration files.

Download the current version of Homegear, the base library (libhomegear-base) and all installed modules from the `download page <https://www.homegear.eu/index.php/Downloads>`_.

Install the packages::
	
	dpkg -i libhomegear-base_XXX.deb
	​apt-get -f install
	​dpkg -i homegear_XXX.deb
	​apt-get -f install
	​dpkg -i homegear-MODULENAME_XXX.deb
	​apt-get -f install

``apt-get -f install`` installs any missing dependencies.

That's it!


Update From Source
******************

Follow the installation instructions under ":ref:`compiling-homegear`". Before doing anything, you should backup your database in "/var/lib/homegear/db.sql" and your settings in "/etc/homegear"!