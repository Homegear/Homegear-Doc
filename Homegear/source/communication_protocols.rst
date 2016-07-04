Communication Protocols
#######################

MQTT
****

MQTT is a publish-subscribe-based communication protocol. For it to work you need to install a MQTT broker like ``mosquitto``. Homegear's MQTT interface features:

* Notification on device variable changes
* Call of all RPC methods
* TLS support
* Support for authentication with user name password
* Support for certificate authentication

Configuration
=============

The MQTT configuration file, ``mqtt.conf``, can be found in Homegear's configuration directory. The following configuration options are known to Homegear:

+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| Option                 | Default Value | Description                                                                                                                            |
+========================+===============+========================================================================================================================================+
| **enabled**            | false         | Set to ``true`` to enable MQTT in Homegear.                                                                                            |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **brokerHostname**     |               | The hostname or IP address of the MQTT message broker to connect to.                                                                   |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **borkerPort**         | 1883          | The port the MQTT message broker listens on.                                                                                           |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **clientName**         | Homegear      | The name of the MQTT client.                                                                                                           |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **homegearId**         |               | The ID of this Homegear instance. Set this to an arbitrary value unique to the Homegear instance.                                      |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **ratain**             | true          | Set to ``true`` to tell the MQTT broker to retain received messages. New clients then receive the last value of a topic on connection. |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **userName**           |               | When set this user name is used to authenticate the Homegear instance.                                                                 |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **password**           |               | When set this password is used to authenticate the Homegear instance.                                                                  |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **enableSSL**          | false         | When ``true`` the connection to the MQTT broker will be TLS encrypted.                                                                 |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **caFile**             |               | Path to the certificate authority's public certificate. This certificate is used to verify the MQTT broker's certificate.              |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **verifiyCertificate** |               | You should always keep this setting set to ``true`` to prevent man-in-the-middle attacks. Only set to ``false`` for debugging.         |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **certPath**           |               | If set, this client certificate is used to authenticate the Homegear instance. You need to set ``keyPath``, too.                       |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+
| **keyPath**            |               | If set, this client certificate is used to authenticate the Homegear instance. You need to set ``certPath``, too.                      |
+------------------------+---------------+----------------------------------------------------------------------------------------------------------------------------------------+

Topics
======

Variable Changes
----------------

Variable state changes are published to::

	homegear/HOMEGEAR_ID/event/PEERID/CHANNEL/VARIABLE_NAME

``HOMEGEAR_ID`` is replaced with the value of ``homegearId`` set in ``mqtt.conf``. PEERID, CHANNEL and VARIABLE_NAME are replaced with the data of the changed variable. The payload is the JSON-encoded value of the variable.

Let's say ``homegearId`` is ``0123-4567``, the peer ID is ``155``, the channel is ``3``, the variable name is ``STATE`` and the value is ``true``. Then the topic is::

	homegear/0123-4567/155/3/STATE

and the value is::

	[true]


Set Variable
------------

The topic to set a variable is::

	homegear/HOMEGEAR_ID/value/PEERID/CHANNEL/VARIABLE_NAME

The payload needs to be the JSON-encoded value. Let's say ``homegearId`` again is ``0123-4567``, the peer ID is ``155``, the channel is ``3``, the variable name is ``STATE`` and you want to change the value to ``true``. Then the topic you need to publish to is::

	homegear/0123-4567/value/155/3/STATE

and the payload needs to be::

	[true]


Set Configuration Parameters
----------------------------

The topic to set configuration parameters is::

	homegear/HOMEGEAR_ID/config/PEERID/CHANNEL/PARAMETERSET_TYPE

The payload needs to be the JSON-encoded value object containing the key value pairs of the configuration parameters to set. Let's say ``homegearId`` is ``0123-4567``, the peer ID is ``155``, the channel is ``0``, the parameter set type is ``MASTER`` and you want to change the parameters ``LANGUAGE_CODE`` to ``EN`` and ``CITY_ID`` to ``London``. Then the topic you need to publish to is::

	homegear/0123-4567/config/155/0/MASTER

and the payload is::

	{
		"LANGUAGE_CODE": "EN",
		"CITY_ID": "London"
	}

RPC Methods
-----------

The topic to call RPC methods is::

	homegear/HOMEGEAR_ID/rpc

The payload needs to be the JSON-RPC encoded method call. Let's say you want to change the log level to ``3``, the payload would look like::

	{ "jsonrpc": "2.0", "id": 123, "method": "logLevel", "params": [3]}

The RPC response is published to::

	homegear/HOMEGEAR_ID/rpcResult

``id`` can be used to identify the result.

Let's say you want to get the current Homegear version, then the payload to publish to ``homegear/HOMEGEAR_ID/rpc`` would look like::

	{ "jsonrpc": "2.0", "id": 123, "method": "logLevel", "params": []}

Then the result Homegear publishes to ``homegear/HOMEGEAR_ID/rpcResult`` is::

	{"id":124,"method":"logLevel","result":3}

As you can see, ``id`` is set to ``124`` as defined in the request.