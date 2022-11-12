forwarding (data plane) - router-local action of transfering a packet from input link interface to appropriate output link interface
	destination-based forwarding
	generalized forwarding

routing (control plane) - network-wide process determinating end-to-end paths of packets from src to dst

traditionally `routing algorithms` (control plane ofc) are working per-router and communicationg with each other via `routing protocols`
in SDN (`software defined networking`) data & control plane are explicitly separated - control plane functions as separate servce typically in separate remote "controller"

Internet network layer provide only *best-effort service*
ATM network layer provide *guaranteed in-order delay*, *bounded delay*, *guaranteed minimal bandwidth*

##### generic router architecture

![forwarding router](./img/forwarding_router.png)
- input ports - L1 functions -> L2 functions -> forward table lookup -> packet switching via `switch fabric`
- `switching fabric` - piece of hardware connecting input interface with output interface
- output ports - stores&transmits packets from `switching fabric` to outgoing links via L2&L1 functions
  if `bidirectional port` - input&output ports usually paired on the same `line card`
- routing processor - control plane functions:
	  traditional routers:
		  - execute forwarding tables
		  - maintain forward tables & attached link state info
		  - compute forwarding tables
	  SDN routers:
		  - communicating with `remote controller` (i.a. receive forwarding tables)
		  - installing forwarding table entries to input ports
there are also some other network management fctions

