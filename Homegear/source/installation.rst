Installation
============

.. highlight:: bash

System Requirements
-------------------

Hardware Recommendations
^^^^^^^^^^^^^^^^^^^^^^^^

Homegear runs on pretty much any computer running GNU/Linux. The only requirement is a minimum memory size of 256 MB. Recommended is 1 GB. The memory limits the number of threads that can run simultaneously. Some devices don't need threads and some devices do. So depending on what you want to do, you need more or less memory. 1 GB should be more than enough for all normal installations though.

.. note:: It is recommended to use a battery backup for your system so there is no data corruption in case of a power blackout.


Software Requirements
^^^^^^^^^^^^^^^^^^^^^

Homegear should run on all current GNU/Linux distributions. With small changes it can also run on BSD and Mac OS systems.

.. warning:: Do not run Homegear in a virtual machine as packet timing is very inaccurate.


Installing from Repository
--------------------------

If you are using Debian, Raspbian or Ubuntu you can install Homegear from repository. Please follow the instructions for your system.


Debian
^^^^^^

Packages are provided for Debian 7 (Wheezy) and Debian 8 (Jessie). Supported architectures are i386, amd64, armel and armhf.

.. warning:: Don't use the Debian repositories for Raspbian. Use the Raspbian repository instead.


Debian 7 (Wheezy)
"""""""""""""""""

Please run the following commands as root::

	wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
	​echo 'deb https://homegear.eu/packages/Debian/ wheezy/' >> /etc/apt/sources.list.d/homegear.list 
	​apt-get update
	​apt-get install homegear

After installing Homegear, install any family modules you like. To install all family modules available, run::

	apt-get install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos


Debian 8 (Jessie)
"""""""""""""""""

Please run the following commands as root::

	wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
	​echo 'deb https://homegear.eu/packages/Debian/ jessie/' >> /etc/apt/sources.list.d/homegear.list 
	​apt-get update
	​apt-get install homegear

After installing Homegear, install any family modules you like. To install all family modules available, run::

	apt-get install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos


Raspbian
^^^^^^^^

Packages are provided for Raspbian 7 (Wheezy) and Raspbian 8 (Jessie).


Raspbian 7 (Wheezy)
"""""""""""""""""""

Please run the following commands as root::

	wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
	​echo 'deb https://homegear.eu/packages/Raspbian/ wheezy/' >> /etc/apt/sources.list.d/homegear.list 
	​apt-get update
	​apt-get install homegear

After installing Homegear, install any family modules you like. To install all family modules available, run::

	apt-get install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos


Raspbian 8 (Jessie)
"""""""""""""""""""

Please run the following commands as root::

	wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
	​echo 'deb https://homegear.eu/packages/Raspbian/ jessie/' >> /etc/apt/sources.list.d/homegear.list 
	​apt-get update
	​apt-get install homegear

After installing Homegear, install any family modules you like. To install all family modules available, run::

	apt-get install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos


Ubuntu
^^^^^^

Packages are provided for Ubuntu 14.04 (Trusty Tahr) and Ubuntu 15.10 (Wily Werewolf).


Ubuntu 14.04 (Trusty Tahr)
""""""""""""""""""""""""""

Please run the following commands as root::

	wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
	​echo 'deb https://homegear.eu/packages/Ubuntu/ trusty/' >> /etc/apt/sources.list.d/homegear.list 
	​apt-get update
	​apt-get install homegear

After installing Homegear, install any family modules you like. To install all family modules available, run::

	apt-get install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos


Ubuntu 15.10 (Wily Werewolf)
""""""""""""""""""""""""""""

Please run the following commands as root::

	wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
	​echo 'deb https://homegear.eu/packages/Ubuntu/ wily/' >> /etc/apt/sources.list.d/homegear.list 
	​apt-get update
	​apt-get install homegear

After installing Homegear, install any family modules you like. To install all family modules available, run::

	​apt-get install homegear-homematicbidcos homegear-homematicwired homegear-insteon homegear-max homegear-philipshue homegear-sonos


Manually Install Debian/Raspbian/Ubuntu Package
-----------------------------------------------

Download the correct packages from the `Homegear download page <https://www.homegear.eu/index.php/Downloads>`_. You need at least the packages ``libhomegear-base`` and ``homegear``. Additionally download all family module packages you want to use. Then as root install the packages using dpkg::

	dpkg -i libhomegear-base_XXX.deb
	​apt-get -f install
	​dpkg -i homegear_XXX.deb
	​apt-get -f install
	​dpkg -i homegear-MODULENAME_XXX.deb
	​apt-get -f install

The "apt-get -f install" installs any missing dependencies.


Raspbian Image
--------------

If you want to use Homegear on a Raspberry Pi, the easiest way is to `download the Raspberry Pi image <https://www.homegear.eu/index.php/Downloads>`_ and write it to a SD card.

Follow the instructions on `elinux.org <http://elinux.org/RPi_Easy_SD_Card_Setup#Flashing_the_SD_Card_using_Windows>`_ (for Windows, Mac and GNU/Linux) to get the image onto your SD card.

.. note:: Username is ``pi``, password is ``raspberry``

SSH is enabled on port 22, so you can use a SSH client (e. g. PuTTY) to login and you don't need to connect a display and keyboard. You can try logging in using the hostname ``homegearpi`` or you need to look the IP address up in your DHCP server (or router). When you first login, the Raspberry Pi configuration tool will start.

Compiling from Source
---------------------

Compiling Current GitHub Source Using Docker Image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The easiest way to compile Homegear from source is by using Docker. Docker images are provided for Debian 7 (Wheezy; amd64, i386, armhf, armel), Debian 8 (Jessie; amd64, i386, armhf, arm64, armel), Raspbian Wheezy, Raspbian Jessie, Ubuntu 14.04 (Trusty Tahr; amd64, i386, armhf, arm64) and Ubuntu 15.10 (Wily Werewolf; amd64, i386, armhf, arm64). Start the Docker image by running::

	docker run -it -e HOMEGEARBUILD_SHELL=1 homegear/build:TAG

Replace "TAG" with one of the tags from `the repository <https://hub.docker.com/r/homegear/build/tags/>`_ (e. g. debian-jessie-amd64). The environment variable needs to be set to avoid being asked for information about a server to upload the created packages to. To speed up compilation you can also set ``HOMEGEARBUILD_THREADS`` to the number of CPU cores of your system.

In the container execute::

	/build/CreateDebianPackageNightly.sh

When done, you can find the created Debian packages in the directory ``/build``.


Manually Compiling Homegear
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _compiling-php:

Compiling PHP
"""""""""""""

**Debian / Ubuntu / Raspbian**

For all systems Homegear is available as Debian package you can get the needed PHP library and header files by installing "php7-homegear-dev" using apt::

	apt-get install php7-homegear-dev


**Prerequisites**

For all other systems you need to compile PHP 7 from source. First of all, install the prerequisites.


*openSUSE Leap*

Execute::

	zypper install autoconf gcc gcc-c++ libxml2-devel libopenssl-devel enchant-devel gmp-devel libmcrypt-devel libedit-devel


**Compiling**

Download the PHP source code from the `PHP download page <http://php.net/downloads.php>`_. Then extract the package::

	tar -zxf php-7.X.X.tar.gz

or::

	tar -jxf php-7.X.X.tar.bz2

Change into the subdirectory "ext" within the extracted directory::

	cd php-7.X.X/ext

Download the current version of pthreads from `GitHub <https://github.com/krakjoe/pthreads/releases>`_, extract it and rename the extracted folder into "pthreads"::

	wget https://github.com/krakjoe/pthreads/archive/vX.X.X.tar.gz
	​tar -zxf vX.X.X.tar.gz
	​rm vX.X.X.tar.gz
	​mv pthreads-X.X.X pthreads

We need to allow pthreads to be loaded in Homegear::

	sed -i 's/{ZEND_STRL("cli")}/{ZEND_STRL("homegear")}/g' pthreads/php_pthreads.c

Change into the parent directory and execute autoconf::

	cd ..
	autoconf

Execute the configure script. The lines before the script are needed to get the target system::

	target="$(gcc -v 2>&1)"
	​strpos="${target%%Target:*}"
	​strpos=${#strpos}
	​target=${target:strpos}
	​target=$(echo $target | cut -d ":" -f 2 | cut -d " " -f 2)
	​./configure  --prefix /usr/share/homegear/php --enable-embed=static --with-config-file-path=/etc/homegear --with-config-file-scan-dir=/etc/homegear/php.conf.d --includedir=/usr/include/php7-homegear --libdir=/usr/share/homegear/php --libexecdir=${prefix}/lib --datadir=${prefix}/share --program-suffix=-homegear --sysconfdir=/etc/homegear --localstatedir=/var --mandir=${prefix}/man --disable-debug --disable-rpath --with-pic --with-layout=GNU --enable-bcmath --enable-calendar --enable-ctype --enable-dba --without-gdbm --without-qdbm --enable-inifile --enable-flatfile --enable-dom --with-enchant=/usr --enable-exif --with-gettext=/usr --with-gmp=/usr/include/$target --enable-fileinfo --enable-filter --enable-ftp --enable-hash --enable-json --enable-pdo --enable-mbregex --enable-mbregex-backtrack --enable-mbstring --disable-opcache --enable-phar --enable-posix --with-mcrypt --enable-mysqlnd --enable-mysqlnd-compression-support --with-zlib-dir=/usr --with-openssl --with-libedit=/usr --enable-libxml --enable-session --enable-simplexml --enable-pthreads --with-xmlrpc --enable-soap --enable-sockets --enable-tokenizer --enable-xml --enable-xmlreader --enable-xmlwriter --with-mhash=yes --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-zip --disable-cli --disable-cgi --enable-pcntl --enable-maintainer-zts

If dependencies are missing, install them and run the configure script again until it finishes successfully. You can also remove dependencies, if not needed. When done, run::

	make && make install
	cp /usr/share/homegear/php/lib/libphp7.a /usr/lib/libphp7-homegear.a

Compiling Homegear
""""""""""""""""""

**Prerequisites**

First install all dependencies:

* PHP 7 devel and static library (see :ref:`compiling-php`)
* SQLite 3 devel
* Readline 6 devel
* Libgpg-error devel
* GnuTLS devel
* Libgcrypt devel
* Libxslt devel (needed by PHP)


*Debian / Raspbian / Ubuntu*

On Debian, Raspbian or Ubuntu run::

	apt-get install libsqlite3-dev libreadline6-dev libgpg-error-dev libgnutls28-dev libxslt-dev (libgcrypt11-dev or libgcrypt20-dev)


*openSUSE Leap*

On openSUSE Leap run::

	zypper install libtool libgnutls-devel libgpg-error-devel sqlite3-devel libgcrypt-devel libxslt-devel


**Compiling**

Then download Homegear's base library and extract it::

	wget https://github.com/Homegear/libhomegear-base/archive/master.zip
	​unzip master.zip
	​rm master.zip

Change into the extracted directory and run ``makeRelease.sh`` or ``makeDebug.sh``. You can pass the number of build threads to the script to speed up compilation::

	cd libhomegear-base-master
	./makeRelease.sh 4

The same for Homegear::

	wget https://github.com/Homegear/Homegear/archive/master.zip
	​unzip master.zip
	​rm master.zip
	​cd Homegear-master
	​./makeRelease.sh 4

Repeat these steps for all family modules you want to compile.


**Configuration**

First add a user named homegear::

	useradd --system -U --no-create-home homegear

Copy the default configuration files::

	cp -R misc/Config\ Directory /etc/homegear

Now setup all necessary directories ::

	mkdir /var/log/homegear
	​chmod 750 /var/log/homegear
	​chown homegear:homegear /var/log/homegear
	​chmod 750 /var/lib/homegear
	​chown homegear:homegear /var/lib/homegear

and create the certificates needed for SSL/TLS encryption::

	openssl genrsa -out /etc/homegear/homegear.key 2048
	​openssl req -batch -new -key /etc/homegear/homegear.key -out /etc/homegear/homegear.csr
	​openssl x509 -req -in /etc/homegear/homegear.csr -signkey /etc/homegear/homegear.key -out /etc/homegear/homegear.crt
	​rm /etc/homegear/homegear.csr
	​chown homegear:homegear /etc/homegear/homegear.key
	​chmod 400 /etc/homegear/homegear.key
	​openssl dhparam -check -text -5 1024 -out /etc/homegear/dh1024.pem
	​chown homegear:homegear /etc/homegear/dh1024.pem
	​chmod 400 /etc/homegear/dh1024.pem

**First Start**

Now try to start Homegear with ::

	homegear -u homegear -g homegear -d

and watch the log file with ::

	tail -n 1000 -f /var/log/homegear/homegear.log

to see, if everything is working fine.