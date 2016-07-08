Command Line Interface (CLI)
############################

.. highlight:: bash

Homegear's CLI enables you to:

* Create, update or delete users
* Load, unload or reload modules
* Change the debug level at runtime
* Execute scripts or one line script (PHP) commands
* List, pair, unpair, search or delete devices

Starting the CLI
****************

To start the CLI just enter::

	homegear -r

You need to have permissions to write to the CLI socket. In most cases for that to be the case you need to be root or a member or the group ``homegear``.

.. warning:: If the connections fails, also check if Homegear is running, e. g. with ``ps -A | grep homegear``.

To show a list of the available commands enter ``help``::

	> help
	List of commands (shortcut in brackets):

	For more information about the individual command type: COMMAND help

	debuglevel (dl)     Changes the debug level
	runscript (rs)      Executes a script with the internal PHP engine
	runcommand (rc)     Executes a PHP command
	scriptcount (sc)    Returns the number of currently running scripts
	rpcservers (rpc)    Lists all active RPC servers
	rpcclients (rcl)    Lists all active RPC clients
	threads             Prints current thread count
	users [COMMAND]     Execute user commands. Type "users help" for more information.
	families [COMMAND]  Execute device family commands. Type "families help" for more information.
	modules [COMMAND]   Execute module commands. Type "modules help" for more information.

To get more information about a specific command, you can always type ``COMMAND help``, e. g.::

	> runcommand help
	Description: Executes a PHP command. The Homegear object ($hg) is defined implicitly.
	Usage: runcommand COMMAND

	Parameters:
	  COMMAND:              The command to execute. E. g.: $hg->setValue(12, 2, "STATE", true);

