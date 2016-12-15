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

Example usage of the CLI
************************

In this example, a simple script should be executed, when a window is opened. The procedure is described here step by step.

First, copy your wanted script to ``/var/lib/homegear/scripts/`` and make your script executable and accessable by the user homegear. Then start the CLI::

	homegear -r

Inspect the connected devices::

    rc print_r($hg->listDevices())

The output of this command lists an array of all devices with all attributes. Search for the Device with type ``SHUTTER_CONTACT``. The section may look like::

   [5] => Array
        (
            [ADDRESS] => NEQ0756970:1
            [AES_ACTIVE] => 0
            [CHANNEL] => 1
            [DIRECTION] => 1
            [FAMILY] => 0
            [FLAGS] => 1
            [ID] => 2
            [INDEX] => 1
            [LINK_SOURCE_ROLES] => KEYMATIC SWITCH WINDOW_SWITCH_RECEIVER WINMATIC
            [LINK_TARGET_ROLES] => 
            [PARAMSETS] => Array
                (
                    [0] => MASTER
                    [1] => VALUES
                    [2] => LINK
                )

            [PARENT] => NEQ0756970
            [PARENT_TYPE] => HM-Sec-SC-2
            [TYPE] => SHUTTER_CONTACT
            [VERSION] => 16
        )

The relevant information for adressing this device is the [ID] and the [CHANNEL].
Now inspect the possible values provided by this device::

    rc print_r($hg->getParamset(2,1,"VALUES"))

The first two parameters of this command are the [ID], the [CHANNEL] of the related device.
The output may be::

    Array
    (
        [ERROR] => 0
        [INSTALL_TEST] => 
        [LOWBAT] => 
        [STATE] => 1
    )
    
The variable [STATE] contains the status of the shutter contact. It shows "1" for true or no value for false.

Now install the event::

    rc $hg->addEvent(array("TYPE" => 0, "ID" => "MyWindowEvent", "PEERID" => 2, "PEERCHANNEL" => 1, "TRIGGER" => 8, "TRIGGERVALUE" => true, "VARIABLE" => "STATE", "EVENTMETHOD" => "runScript", "EVENTMETHODPARAMS" => array("mywindowscript.sh") ))

The command ist built out of the following variables:

TYPE
    Set to "0" for a triggered event. For a description of this variable, see the page `<https://www.homegear.eu/index.php/XML_RPC_EventDescription>`_

ID
    Choose any string for a good identification of the created event.

PEERID
    This is the [ID] , that is listed in the attributes array of the shutter contact.

PEERCHANNEL
    This is the [CHANNEL] , that is listed in the attributes array of the shutter contact.

TRIGGER
    Set to "8" for executing the script on particular events. For a description of this variable, see the page `<https://www.homegear.eu/index.php/XML_RPC_EventDescription>`_

TRIGGERVALUE
    set to "true" for executing the script when the [STATE] variable of the shutter contact changes its value to "true".

VARIABLE
    set to "STATE" to evaluate the variable [STATE]

EVENTMETHOD
    set to "runScript" for running a script in the directory /var/lib/homegear/scripts/ . Other possible commands are listed here: `<https://www.homegear.eu/index.php/XML_RPC_Method_Reference#Event_functions>`_

EVENTMETHODPARAMS
    This array contains the filename of the script and arguments, that are passed to the script.

Now, run::

    rc print_r($hg->listEvents())

for the complete list of activated scripts. There shold be an event like::

    [2] => Array
        (
            [ENABLED] => 1
            [EVENTMETHOD] => runScript
            [EVENTMETHODPARAMS] => Array
                (
                    [0] => mywindowscript.sh
                )

            [ID] => MyWindowEvent
            [LASTRAISED] => 0
            [LASTVALUE] => 
            [PEERCHANNEL] => 1
            [PEERID] => 2
            [TRIGGER] => 8
            [TRIGGERVALUE] => 1
            [TYPE] => 0
            [VARIABLE] => STATE
        )

The script "mywindowscript.sh" will now be executed every time, the shutter contact changes its [STATE] variable to true.
