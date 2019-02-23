Configuration
#############

To connect Homegear with a Hue bridge, the bridge need to be configured with the Hue app first, and at least one lamp needs to be connected.

.. note:: No accessory (like motion sensor) can be connected with the bridge using the app, if not at least one lamp is connected.

After initial setup, start the homegear console::

 homegear -r

Thereafter, select the Philips Hue family and start searching for the bridge::

  family select 5 
  search

Press the pair button on bridge to allow homegear connecting to it.

Now you can list and also use connected devices:

  ls
  
.. note:: Motion detectors are not supported, as the bridge doesn't push their states.
