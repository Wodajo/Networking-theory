Ethernet - collection of network protocols&standards
defined in IEEE (Institute od Electrical and Electronics Engineers) 802.3 standard in 1983



Cables:

Copper-based
| IEEE standard | common name      | speed    | informal name | max. length |
|:-------------:|:----------------:|:--------:|:-------------:|:-----------:|
| 802.3i        | Ethernet         | 10 Mbps  | 10BASE-T      | 100m.       |
| 802.3u        | Fast Ethernet    | 100 Mbps | 100BASE-T     | 100m.       |
| 802.3ab       | Gigabit Ethernet | 1 Gbps   | 1000BASE-T    | 100m.       |
| 802.3an       | 10 Gig Ethernet  | 10 Gbps  | 10GBASE-T     | 100m.       |
e.g. 100BASE-t -> 100 (speed [Mbps]),  BASE (baseband signaling), T - twisted pair

- UTP unshielded twisted pair: - have 4 pairs of wires
	- 10BASE-T & 100BASE-T - use 2 pairs (4 wires)
	PC, fw & router NIC <---> swich NIC: - needs [straight-through cable]
		TX (transmit) pins 1&2 ---> RX (receive) pins 1&2
		RX pins 3&6 <--- TX pins 3&6
	PC <-> PC, router <-> router, router <-> PC, swich <-> swich: - needs [crossover cable]
		pins 1&2 (PC, fw&router - TX, swich - RX) ---> pins 3&6 (PC, fw&router - RX, swich - TX)
	[auto MDI-X] - can detect on what pins neighbour NIC is TX&RX and adjust  on what pins they're TX&RX. Old devices might not support auto MDI-X
	
	- 1000BASE-T & 10GBASE-T - use 4 pairs (8 wires)
		Each pin pairs (1&2, 3&6, 4&5, 7&8) are bidirectional

Fiber
| IEEE standard | informal name | speed  | max. length         | cable type               |
|---------------|---------------|--------|---------------------|--------------------------|
| 802.3z        | 1000BASE-LX   | 1Gbps  | 550m (MM), 5km (SM) | Multimode or Single-mode |
| 802.3ae       | 10GBASE-SR    | 10Gbps | 400m                | Multimode                |
| 802.3ae       | 10GBASE-LR    | 10Gbps | 10km                | Single-mode              |
| 802.3ae       | 10GBASE-ER    | 10Gbps | 30km                | Single-mode              |
(there is more)

- use one cable to RX and one to TX
	- Multi-mode fiber:
		- wider core diameter
		- allows multiple angles (modes) of light to enter
		- longer cables than UTP BUT shorter than single-mode fiber
		- cheaper than single-mode fiber (cheaper LED-based SFP transmitters)
	- Single-mode fiber:
		- narrower core diameter
		- single angle (mode)
		- longer than both UTP & multimode fiber
		- laser-based transmitter (expensive)

Ports:
- RJ-45 (Registered Jack) - used with copper ethernet cable
- SFP Transceiver (Small Form-Factor Pluggable) - insert them into ports for fiber port, and fiber into transceiver