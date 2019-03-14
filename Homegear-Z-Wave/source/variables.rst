Variables
#########

.. highlight:: php

To get the list of available variables in the channel 1, execute::

    homegear -e rc 'print_v($hg->getParamset(<peer ID>, 1, "VALUES"));'

You can set a variable from the command line by executing::

    homegear -e rc '$hg->setValue(<peer ID>, 1, <variable>, <value>);'

To retrieve the value of a variable, execute::

    homegear -e rc 'print_v($hg->getValue(<peer ID>, 1, <variable>));'
