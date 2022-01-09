Installation
############

.. highlight:: bash

System Requirements
*******************

See :ref:`hardware-and-software-requirements`.


Installing from Repository
**************************

If you are using Debian, Raspbian, or Ubuntu, you can install Homegear from the repository. To do so, please follow the instructions for your system on https://homegear.eu/downloads.html.


Raspberry Pi OS Image
*********************

The easiest way to use Homegear on a Raspberry Pi is to `download the Raspberry Pi image <https://www.homegear.eu/downloads.html>`_ and write it to an SD card.

Follow the instructions on `elinux.org <http://elinux.org/RPi_Easy_SD_Card_Setup#Flashing_the_SD_Card_using_Windows>`_ to transfer the image to your SD card (for Windows, Mac, and GNU/Linux).

.. note:: The username is ``pi``, and the password is ``raspberry``.

Because SSH is enabled on port 22, you can use an SSH client (such as PuTTY) to log in, and you don't need to connect a display or a keyboard. You can try logging in using the hostname ``homegearpi``. Alternatively, you would need to look up the IP address of your DHCP server (or router). The first time you log in, the Raspberry Pi configuration tool will start.


Compiling from Source
*********************


Compiling Current GitHub Source Using Docker Image
==================================================

The easiest way to compile Homegear from the source is by using Docker. Docker images are provided for Debian 8 (Jessie; amd64, i386, armhf, arm64, armel), Debian 9 (Stretch; amd64, i386, armhf, arm64), Raspbian Jessie, Raspbian Stretch, Ubuntu 16.04 (Xenial Xerus; amd64, i386, armhf, arm64), and Ubuntu 18.04 (Bionic Beaver; amd64, i386, armhf, arm64). Start the Docker image by running the following command::

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

Homegear's PHP 8 library is available for all systems as a Debian package. You can get the required PHP library and header files by installing "php8-homegear-dev" using apt from Homegear's stable repository::

	apt-get install php8-homegear-dev


Prerequisites
^^^^^^^^^^^^^

For all other systems, you need to compile PHP 8 from the source. First of all, you need to install the prerequisites.


openSUSE Leap
"""""""""""""

Execute::

	zypper install autoconf gcc gcc-c++ libxml2-devel libopenssl-devel enchant-devel gmp-devel libmcrypt-devel libedit-devel


Compiling
^^^^^^^^^

.. warning:: Homegear requires at least PHP 7.2 as ZTS is broken in PHP 7.0 and 7.1. Recommended is PHP 8.

Download the PHP source code from the `PHP download page <http://php.net/downloads.php>`_. Then extract the package::

	tar -zxf php-8.X.X.tar.gz

or::

	tar -jxf php-8.X.X.tar.bz2

Switch to the subdirectory "ext" within the extracted directory::

	cd php-8.X.X/ext

Clone the current version of parallel from `GitHub <https://github.com/krakjoe/parallel>`_::

	git clone https://github.com/krakjoe/parallel.git parallel
	cd parallel
	git checkout release

Switch to the parent directory::

	cd ../..

Execute autoconf::

	autoconf

Execute the configure script::

	./configure  --prefix /usr/share/homegear/php --enable-embed=static --with-config-file-path=/etc/homegear --with-config-file-scan-dir=/etc/homegear/php.conf.d --includedir=/usr/include/php8-homegear --libdir=/usr/share/homegear/php --libexecdir=${prefix}/lib --datadir=${prefix}/share --program-suffix=-homegear --sysconfdir=/etc/homegear --localstatedir=/var --mandir=${prefix}/man --disable-debug --disable-rpath --with-pic --with-layout=GNU --enable-bcmath --enable-calendar --enable-ctype --enable-dba --without-gdbm --without-qdbm --enable-inifile --enable-flatfile --enable-dom --with-enchant=/usr --enable-exif --with-gettext=/usr --with-gmp --enable-fileinfo --enable-filter --enable-ftp --enable-json --enable-pdo --enable-mbregex --enable-mbstring --disable-opcache --enable-phar --enable-posix --with-mysqli=mysqlnd --with-zlib-dir=/usr --with-openssl --with-libedit=/usr --enable-session --enable-simplexml --enable-parallel --with-xmlrpc --enable-soap --enable-sockets --enable-tokenizer --enable-xml --enable-xmlreader --enable-xmlwriter --with-mhash=yes --enable-sysvmsg --enable-sysvsem --enable-sysvshm --disable-cli --disable-cgi --enable-pcntl --enable-maintainer-zts

If dependencies are missing, install them and run the configure script again until it finishes successfully. You can also remove dependencies, if they are not needed. When this is done, run::

	make && make install
	cp /usr/share/homegear/php/lib/libphp8.a /usr/lib/libphp8-homegear.a


.. _compiling-nodejs:

Compiling Node.js
-----------------

Debian / Ubuntu / Raspbian
^^^^^^^^^^^^^^^^^^^^^^^^^^

Homegear's Node.js library is available for all systems as a Debian package. You can get the required PHP library and header files by installing "nodejs-homegear" using apt from Homegear's stable repository::

	apt-get install nodejs-homegear

Compiling
^^^^^^^^^

For all other systems, you need to compile Node.js from the source::

	mkdir build
	cd build
	# Replace with the current version
	wget https://github.com/nodejs/node/archive/v15.5.0.tar.gz
	tar -zxf v*.tar.gz
	cd node*
	sed -i 's/libnode/libnodejs-homegear/g' node.gyp
	sed -i 's/libnode/libnodejs-homegear/g' deps/npm/node_modules/node-gyp/lib/configure.js
	sed -i "s/output_file = 'node'/output_file = 'nodejs-homegear'/g" tools/install.py		
	./configure --shared --prefix /usr
	make install


Compiling Homegear
------------------


Prerequisites
^^^^^^^^^^^^^

First, install all dependencies:

* Libtool
* Automake
* PHP 8 devel and static library (see :ref:`compiling-php`)
* Node.js shared library (see :ref:`compiling-nodejs`)
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
	unzip master.zip
	rm master.zip

Switch to the extracted directory and run ``makeRelease.sh`` or ``makeDebug.sh``. You can pass the number of build threads to the script to speed up compilation::

	cd libhomegear-base-master
	./makeRelease.sh 4

Then do the same for Homegear's node library::

	wget https://github.com/Homegear/libhomegear-node/archive/master.zip
	unzip master.zip
	rm master.zip
	cd libhomegear-node-master
	./makeRelease.sh 4

For Homegear::

	wget https://github.com/Homegear/Homegear/archive/master.zip
	unzip master.zip
	rm master.zip
	cd Homegear-master
	./makeRelease.sh 4

And the core nodes::

	wget https://github.com/Homegear/homegear-nodes-core/archive/master.zip
	unzip master.zip
	rm master.zip
	cd homegear-nodes-core-master
	./makeRelease.sh 4

You can also compile the optional Homegear Management service::

	wget https://github.com/Homegear/homegear-management/archive/master.zip
	unzip master.zip
	rm master.zip
	cd homegear-management-master
	./makeRelease.sh 4

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
	chmod 750 /var/log/homegear
	chown homegear:homegear /var/log/homegear
	
	mkdir /var/log/homegear-management
	chmod 750 /var/log/homegear-management
	chown homegear:homegear /var/log/homegear-management
	
	mkdir /var/lib/homegear
	chmod 750 /var/lib/homegear
	chown homegear:homegear /var/lib/homegear

Finally, create the certificates required for SSL/TLS encryption::

	openssl genrsa -out /etc/homegear/homegear.key 2048
	openssl req -batch -new -key /etc/homegear/homegear.key -out /etc/homegear/homegear.csr
	openssl x509 -req -in /etc/homegear/homegear.csr -signkey /etc/homegear/homegear.key -out /etc/homegear/homegear.crt
	rm /etc/homegear/homegear.csr
	chown homegear:homegear /etc/homegear/homegear.key
	chmod 400 /etc/homegear/homegear.key
	openssl dhparam -out /etc/homegear/dh1024.pem -check -text -5 1024
	chown homegear:homegear /etc/homegear/dh1024.pem
	chmod 400 /etc/homegear/dh1024.pem


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

If you want to use Homegear Gateways or the Homegear Gateway service, you need to create a certificate authority to create gateway and client certificates. The easiest way to do that is by using Homegear's Managament service. Note that creating the CA this way changes your `/usr/lib/ssl/openssl.cnf`::

    homegear -e rc 'print_v($hg->managementCreateCa());'

This creates a CA in ``/etc/homegear/ca`` in background. It can only be executed once and returns ``true`` on success. To check if the command has finished, execute::

    homegear -e rc 'print_v($hg->managementGetCommandStatus());'

This returns the command output and the exit code. The command has finished if the exit code is other than ``256``. On success the exit code is ``0``.

Next create the client certificate to login into gateways::

    homegear -e rc 'print_v($hg->managementCreateCert("gateway-client"));'

Again this command runs in background and you can check if the command has finished with::

    homegear -e rc 'print_v($hg->managementGetCommandStatus());'
