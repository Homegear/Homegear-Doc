Homegear Documentation
======================

This repository contains the source of the official Homegear documentation. The documentation is hosted at https://homegear.eu/doc.

## Building the Homegear documentation

### Installing the brand

* Install publican
* Change into the publican-Homegear directory and run:
```
publican build --formats xml --langs all --publish
sudo publican install_brand --path path
```
Where path is the path to the Publican Common Content files. On Linux the path typically is: /usr/share/publican/Common_Content.

### Building the documenation

* Change into the documentation source directory (i. e. "Homegear_0.6_Documentation")
* Run:
```
publican build
```
* The documentation can now be found in the directory "tmp".
