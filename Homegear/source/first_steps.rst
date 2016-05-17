First Steps
###########

Make sure, Homegear is up and running before you continue reading this chapter.

How to Interact with Homegear
*****************************

There are two ways to interact with Homegear:

* First Homegear offers a Command Line Interface (CLI). Through the CLI you can do some basic things like creating users, pairing devices or execute one line script code.
* The second way is to control Homegear using one of supported RPC protocols. By default Homegear listens on three ports (defined in /etc/homegear/rpcservers.conf):

+------+--------------------------------------------------------------------+
| Port | Description                                                        |
+======+====================================================================+
| 2001 | No SSL, no authentication                                          |
+------+--------------------------------------------------------------------+
| 2002 | SSL enabled, authentication enabled                                |
+------+--------------------------------------------------------------------+
| 2003 | SSL and authentication enabled                                     |
+------+--------------------------------------------------------------------+

If your client supports SSL and Basic Access Authentication, always connect to port 2003 as it is the most secure. If all your clients support SSL and authentication, disable the first two servers in "/etc/homegear/rpcservers.conf". If you want to access Homegear over the internet, you should always use SSL and authentication.

Each of the three ports supports all RPC protocols. The web server listens on these three ports as well.