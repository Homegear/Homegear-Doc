Variables
#########

.. highlight:: php

To get the list of available variables, execute::

    homegear -e rc 'print_v($hg->getParamset(<peer ID>, 1, "VALUES"));'

To retrieve the value of a variable, execute::

    homegear -e rc 'print_v($hg->getValue(<peer ID>, 1, <variable>));'
