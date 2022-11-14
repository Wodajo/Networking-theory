`eth. frame`:
- header:  26 [bytes] with preamble and SFD, **18** [bytes] without
	- ~~preamble~~[7 bytes]- allows devices synch. receiver clocks. Strikethrough bcos it's usually NOT considered a part of `eth. frame` header
	  10101010 * 7
	- ~~SFD~~ (`Start Frame Delimiter`) [1byte]
	  10101011
	- destination [6 bytes = 12 hex digits]
	- source [6 bytes]
	- type/length [2 bytes]
	  if =< 1500 - length of packet[bytes]
	  if 1536 => - type of packet
	  (usually IPv4 (0x0800 = 2048) or IPv6 (0x86DD = 34525))
- payload (packet) [MTU]
- trailer:
	- FCS (`Frame Check Sequence`) [4 byte] - detect corrupted data with CRC (`Cyclic Redundancy Check`)

min. size of `eth. frame` = 64 [bytes]
min. payload size = 64 - 18 = 46 [bytes]
if payload < 46 -> zeros padding

MAC (`Media Access Control`) aka BIA (`Burned-In Address`):
OUI (`Organizationally Unique Identifier`) - first 3 bytes (6 hex digits) of MAC

`MAC address table` - map MAC to port on switches
switch dynamically populate MAC address table form frame source field (removed after 5 min of inactivity)
THEREFORE - ping should populate MAC address table
(ICMP Echo request - dynamic MAC of sender, ICMP Echo reply - dynamic MAC of receiver)

unknown unicast frame - dynamic MAC (MAC address table populated based on frame source field) -> flood LAN
known unicast frame - forward to destination


ARP (`Address Resolution Protocol`):
ARP request - broadcast (destinated at FFFF:FFFF:FFFF)
ARP reply - unicast

`ARP table`: - map MAC to IP
static - default entry
dynamic - learned via `ARP reply`

ping - uses ICMP Echo request & ICMP Echo reply (part of IP stack) -> rely on ARP to get MAC *before* actually sending ICMP Echo request
MAC is needed to forward `eth. frames` to node with targeted IP address.
That's why first ping fails (timeout during ARP resolution) 