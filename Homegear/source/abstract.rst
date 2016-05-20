Abstract
########

Homegear is a free and open source program that you can use to interface your home automation devices or services that use your home automation software or scripts. It features the following interfaces for controlling your devices, all of which have SSL support:

* XML-RPC
* Binary-RPC
* JSON-RPC
* MQTT
* WebSockets with PHP session authentication
* HTTP (GET and POST) 

If needed, new interfaces can easily be added to the source code. Homegear also features:

* A built-in, rich-featured web server with PHP 7 and IP cam proxy support. Together with WebSockets and the script engine, you can easily create web pages to bidirectionally interact with all devices known to Homegear.
* A built-in script engine using PHP 7:

	* All devices and device functions are directly accessible.
	* All PHP modules can be used:

		* Thread support using the PHP module "pthreads"
		* Low level peripheral support:
		
			* You can directly access serial devices, I²C devices, and GPIOs.
			* You are immediately notified about new data and GPIO state changes. No polling is necessary.
			* Using threads, you can implement bidirectional and event-driven communication. 
	* A base library that you can use to easily implement your own device families
	* XML device description files with PHP script support so you can easily implement individual devices
	* Support for customized licensing modules that support module online activation, license verification, and script encryption

Homegear is written to make integration of new devices as easy as possible and to make them accessible using all of the above interfaces.