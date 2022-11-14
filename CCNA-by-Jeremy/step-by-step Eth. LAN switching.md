![eth](./img/eth_switch.png)
PC0 (192.168.1.1) want to sent ping to PC4 (192.168.1.4)

PC0:
1. (L3) ping process starts the `next ping request`
   ping process `create ICMP echo request message` & sends it to lower process -> src. IP address is NOT specified. Device sets it to the port's IP address -> dst. IP address is in `the same subnet`. Device sets the next-hop to destination
2. (L2) next-hop IP address is a unicast
   ARP process looks it up in the ARP table -> next-hop IP `address NOT in the ARP table`. ARP process tries to send an ARP request for that IP address and `buffers this packet` -> ARP process constructs an `ARP request` (broadcast MAC address) for the target IP address -> device encapsulates the PDU into an eth. frame
3. (L1) eth. interface sends out frame


Switch0:
1. (L1) eth. interface receive frame
2. (L2) frame src. MAC address does NOT exist in the MAC table. Switch `adds a new MAC entry` to its table
   frame dst. MAC address is broadcast-> Switch processes the frame
   frame's dst. MAC address matches the receiving port's MAC address, the `broadcast address`, or a multicast address -> device decapsulates the PDU from the eth. frame -> it's an ARP frame. ARP process processes it
   active VLAN interface is NOT up. ARP process ignores the frame  ????
3. (L2) broadcast frame -> Switch sends out the frame to all ports in the same VLAN except the receiving port
4. (L1) eth. interfaces sends out frame


PC1:
1. (L1) eth. interface receive frame
2. (L2) frame's dst. MAC address matches the receiving port's MAC address, the `broadcast address`, or a multicast address -> device decapsulates the PDU from the eth. frame -> it's an ARP frame. The ARP process processes it
   ARP frame is a request -> ARP request's target IP address does not match the receiving port's IP address -> ARP process drops the frame

Router:
1. (L1) eth. interface receive frame
2. (L2) frame src. MAC address does NOT exist in the MAC table. Router `adds a new MAC entry` to its table
   frame dst. MAC address is broadcast -> Router processes the frame
   frame's dst. MAC address matches the receiving port's MAC address, the `broadcast address`, or a multicast address. -> device decapsulates the PDU from the eth. frame -> it's an ARP frame. The ARP process processes it
   active VLAN interface is not up. The ARP process ignores the frame  ????
3. (L2) broadcast frame -> Router sends out the frame to all ports in the same VLAN except the receving port
4. (L1) eth. interfaces sends out frame


Switch1: (same as Switch0 and Router)
1. (L1) eth. interface receive frame
2. (L2) frame src. MAC address does NOT exist in the MAC table. Switch `adds a new MAC entry` to its table
   frame dst. MAC address is broadcast-> Switch processes the frame
   frame's dst. MAC address matches the receiving port's MAC address, the `broadcast address`, or a multicast address -> device decapsulates the PDU from the eth. frame -> it's an ARP frame. ARP process processes it
   active VLAN interface is NOT up. ARP process ignores the frame  ????
3. (L2) broadcast frame -> Switch sends out the frame to all ports in the same VLAN except the receiving port
4. (L1) eth. interfaces sends out frame


PC2: (same as PC1)
1. (L1) eth. interface receive frame
2. (L2) frame's dst. MAC address matches the receiving port's MAC address, the `broadcast address`, or a multicast address -> device decapsulates the PDU from the eth. frame -> it's an ARP frame. The ARP process processes it
   ARP frame is a request -> ARP request's target IP address does not match the receiving port's IP address -> ARP process drops the frame

PC3:
1. (L1) eth. interface receive frame
2. (L2) frame's dst. MAC address matches the receiving port's MAC address, the `broadcast address`, or a multicast address -> device decapsulates the PDU from the eth. frame -> it's an ARP frame. The ARP process processes it
   ARP frame is a request -> ARP request's target IP address `matches the receiving port's IP` address -> ARP process` updates the ARP table` with received information.