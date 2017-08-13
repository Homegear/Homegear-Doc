Hardware and Software Requirements
##################################

.. _hardware-and-software-requirements:

Hardware Recommendations
************************

Homegear runs on essentially any computer running GNU/Linux. The only requirement is a minimum memory size of 256 MB. However, we recommend a memory size of 1 GB. The amount of memory limits the number of threads that can run simultaneously. Some devices don't need threads, but others do. Thus, depending on what you want to accomplish and the devices you want to use, you will require more or less memory. 1 GB should be more than enough memory for all normal installations.

.. note:: We recommend using a battery backup for your system so there is no risk of data corruption in case of power failure or a blackout.


Software Requirements
*********************

You can run Homegear on all current GNU/Linux distributions. With small changes to the source code, you can also run it on BSD and Mac OS systems.

.. warning:: Do not run Homegear in a virtual machine because packet timing is very inaccurate.

There are a few recommendations for production systems:

* A readonly file system should be used. This reduces writes to the storage device and increases it's lifetime. Additionally it protects from file system corruption on power loss.
* Processes with high CPU load should be isolated to a dedicated CPU core so Homegear and more importantly kernel modules are not effected by the high load.