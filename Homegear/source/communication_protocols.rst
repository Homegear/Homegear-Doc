Communication Protocols
#######################

.. _mqtt:

MQTT
****

MQTT is a publish-subscribe-based communication protocol. For it to work you need to install a MQTT broker like ``mosquitto``. Homegear's MQTT interface features:

* Notification on device variable changes
* Setting variables and configuration parameters
* Call of all RPC methods
* TLS support
* Support for authentication with username and password
* Support for certificate authentication

Configuration
=============

The MQTT configuration file, ``mqtt.conf``, can be found in Homegear's configuration directory. The following configuration options are known to Homegear:

+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                    | Default Value | Description                                                                                                                                                                         |
+===========================+===============+=====================================================================================================================================================================================+
| **enabled**               | false         | Set to ``true`` to enable MQTT in Homegear.                                                                                                                                         |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **brokerHostname**        |               | The hostname or IP address of the MQTT message broker to connect to.                                                                                                                |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **borkerPort**            | 1883          | The port the MQTT message broker listens on.                                                                                                                                        |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **clientName**            | Homegear      | The name of the MQTT client.                                                                                                                                                        |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **prefix**                | homegear      | The topic prefix to use. All topics start with this prefix. Can be empty.                                                                                                           |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **homegearId**            |               | The ID of this Homegear instance. Set this to an arbitrary value unique to the Homegear instance.                                                                                   |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **retain**                | true          | Set to ``true`` to tell the MQTT broker to retain received messages. New clients then receive the last value of a topic on connection. Variables of type "Action" are not retained. |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **plainTopic**            | true          | Set to ``false`` to disable Homegear publishing value changes in the "plain" topic. It's recommended to do this to save resources if the topic is unneeded.                         |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **jsonTopic**             | true          | Set to ``false`` to disable Homegear publishing value changes in the "json" topic. It's recommended to do this to save resources if the topic is unneeded.                          |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **jsonobjTopic**          | true          | Set to ``false`` to disable Homegear publishing value changes in the "jsonobj" topic. It's recommended to do this to save resources if the topic is unneeded.                       |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **userName**              |               | When set this user name is used to authenticate the Homegear instance.                                                                                                              |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **password**              |               | When set this password is used to authenticate the Homegear instance.                                                                                                               |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **enableSSL**             | false         | When ``true`` the connection to the MQTT broker will be TLS encrypted.                                                                                                              |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **caFile**                |               | Path to the certificate authority's public certificate. This certificate is used to verify the MQTT broker's certificate.                                                           |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **verifiyCertificate**    |               | You should always keep this setting set to ``true`` to prevent man-in-the-middle attacks. Only set to ``false`` for debugging.                                                      |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **certPath**              |               | If set, this client certificate is used to authenticate the Homegear instance. You need to set ``keyPath``, too.                                                                    |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **keyPath**               |               | If set, this client certificate is used to authenticate the Homegear instance. You need to set ``certPath``, too.                                                                   |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **processingThreadCount** | 5             | The number of parallel threads to process incoming MQTT messages. If messages take to long to get processed, increase this number.                                                  |
+---------------------------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Topics
======

Variable Changes
----------------

Variable state changes are published to::

	<prefix>/<homegearId>/plain/PEERID/CHANNEL/VARIABLE_NAME
	<prefix>/<homegearId>/json/PEERID/CHANNEL/VARIABLE_NAME
	<prefix>/<homegearId>/jsonobj/PEERID/CHANNEL

The three topics differ in the way the payload is encoded:

* ``plain`` contains the value as is. E. g.: ``43.7``.
* ``json`` puts the value in a JSON array to be JSON-compliant: ``[43.7]``.
* ``jsonobj`` puts the value into a JSON object. The key is the variable name. ``jsonobj`` contains all variables received from the device within one packet: ``{ "TEMPERATURE": 20.7, "HUMIDITY": 43 }``.

``<prefix>`` is replaced with the value of ``prefix`` and ``<homegearId>`` with the value of ``homegearId`` set in ``mqtt.conf``. PEERID, CHANNEL and VARIABLE_NAME are replaced with the data of the changed variable.

Let's say prefix is ``homegear``, ``homegearId`` is ``0123-4567``, the peer ID is ``155``, the channel is ``3``, the variable name is ``STATE`` and the value is ``true``. Then the topics are::

	1. homegear/0123-4567/plain/155/3/STATE
	2. homegear/0123-4567/json/155/3/STATE
	3. homegear/0123-4567/jsonobj/155/3

and the payloads are::

	1. true
	2. [true]
	3. { "STATE": true }


Set Variable
------------

The topic to set a variable is::

	<prefix>/<homegearId>/set/PEERID/CHANNEL/VARIABLE_NAME

Let's say ``prefix`` is ``homegear``, ``homegearId`` again is ``0123-4567``, the peer ID is ``155``, the channel is ``3``, the variable name is ``STATE`` and you want to change the value to ``true``. Then the topic you need to publish to is::

	homegear/0123-4567/set/155/3/STATE

The payload can have three different formats:

#. Plain value: ``43.7``
#. JSON array with a single entry (to be JSON-compliant): ``[43.7]``
#. JSON object with a single entry. The key needs to be ``value``: ``{ "value": 43.7 }``


Set Configuration Parameters
----------------------------

The topic to set configuration parameters is::

	<prefix>/<homegearId>/config/PEERID/CHANNEL/PARAMETERSET_TYPE

The payload needs to be the JSON-encoded value object containing the key value pairs of the configuration parameters to set. Let's say ``prefix`` is ``homegear``, ``homegearId`` is ``0123-4567``, the peer ID is ``155``, the channel is ``0``, the parameter set type is ``MASTER`` and you want to change the parameters ``LANGUAGE_CODE`` to ``EN`` and ``CITY_ID`` to ``London``. Then the topic you need to publish to is::

	<prefix>/0123-4567/config/155/0/MASTER

and the payload is::

	{
		"LANGUAGE_CODE": "EN",
		"CITY_ID": "London"
	}


RPC Methods
-----------

The topic to call RPC methods is::

	<prefix>/<homegearId>/rpc

The payload needs to be the JSON-RPC encoded method call. Let's say you want to change the log level to ``3``, the payload would look like::

	{ "jsonrpc": "2.0", "id": 123, "method": "logLevel", "params": [3]}

The RPC response is published to::

	<prefix>/<homegearId>/rpcResult

``id`` can be used to identify the result.

Let's say you want to get the current Homegear version, then the payload to publish to ``<prefix>/<homegearId>/rpc`` would look like::

	{ "jsonrpc": "2.0", "id": 123, "method": "logLevel", "params": []}

Then the result Homegear publishes to ``<prefix>/<homegearId>/rpcResult`` is::

	{"id":124,"method":"logLevel","result":3}

As you can see, ``id`` is set to ``124`` as defined in the request.


Binary RPC
**********

Homegear supports a Binary RPC protocol originally used by software systems from eQ-3. The Binary RPC protocol is the fastest of the protocols supported by Homegear. It is used by HomegearLib.NET, the OpenHAB binding, ioBroker and other systems. Homegear uses an improved version of the Binary RPC protocol and features:

* Call of all RPC methods and reception of all RPC events
* TLS support
* Support for authentication with username and password
