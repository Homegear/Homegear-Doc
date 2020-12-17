First Start
###########

On a default installation, you can access Node-BLUE by entering the URL ``https://<Homegear-IP>:2002/nb`` or ``https://<Homegear-IP>:2002/node-blue``. The default username is ``homegear``. The default password can either be found ``/var/lib/homegear/defaultPassword.txt`` or ``/data/homegear-data/defaultPassword.txt`` (for systems with read-only file systems). It can be changed using Homegear's CLI::

    homegear -r
    users update homegear "new password" "1"
