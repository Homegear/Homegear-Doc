Abstract
########

HomeMatic BidCoS is a wireless home automation system by eQ-3. It features a good portfolio of devices and supports AES signing of packets to prevent unauthorized control of the system.

Homegear fully supports HomeMatic BidCoS and in contrast to most implementations available directly communicates with HomeMatic devices. Like this, you can do stuff you can't with the official central:

	* Support for third-party communication modules
	* Better wireless range, because the third-party modules have better antennas
	* Better latency and less packet loss with some communication modules
	* Devices (particularly remotes and motion detectors) communicate bidirectional with Homegear directly after pairing (signaled by the green LED) - no additional steps are needed
	* Homegear supports roaming. It can automatically select the interface with the best reception.
	* Smoke detectors can be switched on and off