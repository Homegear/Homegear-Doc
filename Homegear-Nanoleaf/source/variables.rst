Variables
#########

.. highlight:: php

For the list and description of supported variables visit the `Homegear device reference <https://ref.homegear.eu/family.html?familyLink=nanoleaf&familyName=Nanoleaf>`_.

You can set a variable from the command line by executing::

    homegear -e rc '$hg->setValue(<peer ID>, 1, <variable>, <value>);'


To retrieve the value of a variable, execute::

    homegear -e rc 'print_v($hg->getValue(<peer ID>, 1, <variable>));'
