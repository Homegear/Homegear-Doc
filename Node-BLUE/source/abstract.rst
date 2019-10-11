Abstract
########

Node-BLUE is Homegear's logic engine. The frontend is based on Node-RED (https://nodered.org/). The backend is fully rewritten in C++.

Node-BLUE adds the following features to Node-RED:

* It is multithreaded and uses queues. No single node can make Node-BLUE hang.
* It allows multiple inputs to one node so it is more PLC-like and less scripts are needed.
* It is blazingly fast.
* Nodes can be written in C++ or PHP.
* There are Python and PHP function nodes.
* Short interval timers are possible and available, e. g. for dimming lights.
* It is fully integrated into Homegear, so accessing device variables and Homegear RPC methods is very easy.
