127.0.0.0 - 127.255.255.255 address range for testing "network stack" on local device

ip address class - first octet - mask
- A - 0xxxxxxx - /8
- B - 10xxxxxx - /16
- C - 110xxxxx - /24

If `host portion` of IP address = all 0's -> Network ID
If `host portion` of IP address = all 1's -> Broadcast ID

So if dst. IP is e.g. 192.168.1.255 (on /24 network) -> frame dst. MAC is FFFF.FFFF.FFFF

###### IP packet structure
- header:
	- version [4 bit]
		  0100 - IPv4
		  0110 - IPv6
	- IHL (internet header length) [4 bit] - length of the header in 4byte incrementations (like TCP header HLEN)
	  min. value - 5, 5x4 = 20 bytes
	  max. value - 15, 15x4 = 60 bytes (40 bytes of options)
	- DSCP (differenciated services code point) [6 bits] - used for QoS (Quality of Service) that is for prioritising delay-senstive data
	- ECN (explicit congestion notification) [2 bit]- for `network-assisted congestion control`.
	  router experience congestion (hopefully *before* packet loss) - set `ECN` bit in IP header-> receiver -TCP `ACK` piggybacking `ECE` (`explicit congestion notification echo` bit)-> sender - set `cwnd` = `cwnd`/2 && set `CWR` (`Congestion Window Reduced`) bit in the next TCP header
	- total length field [16 bit = 2 bytes] - L3 header + L4 segment in bytes
	  min. value - 20 (IPv4 header with no encapsulated data)
	- identification field [16 bit = 2 bytes] - all fragments of the same packet have the same value here
	  packet fragmented if larger than MTU (maximum transmission unit)
		  MTU = MSS + TCP&IP header
		  Usually MTU = 1500, MSS = 1460, TCP&IP header = 40
		  Link layer add 18 bits of frame header and FCS, but that's not taken as a part of network layer MTU
	- flags field [3 bits]:
		- bit 0 - reserved, always 0
		- bit 1 - DF (don't fragment bit) - indicate that packet should not be fragmented
		- bit 2 - MF (more fragments bit) - there are more fragments of that packet (set to 0 on the last fragment)
	- fragment offset [13 bit] - position of fragment within fragmented packet
	- TTL [8 bit = 1 byte] - originally should indicate packet lifetime in s
	  practically - indicate "hop count". Each time a packet arrive at router TTL is decreased by 1
	  Current recommended TTL = 64
	- protocol field [8 bit = 1 byte] - protocol of encapsulated L4 PDU e.g.:
	  value of - 6 (TCP), 17 (UDP), 1 (ICMP), 89 (OSFP open shortest path first - a dynamic routing protocol)
	- header checksum [16 bit] - router drops packets with header checksum mismaching one in header
	- src.&dst. address fields [32 bit = 4 bytes]
	- options [0-320 bit = 0-40 bytes]