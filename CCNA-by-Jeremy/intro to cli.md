may be a physical port (e.g. `RJ-45`, `USB mini B`)

`RJ-45` use `Rollovercable`  
pins pairs:  1-8,  2-7,  3-6,  4-5,  5-4,  6-3,  7-2,  8-1

You need terminal emulator - `PuTTy` is a popular choice. If connected physically use "Serial" option.  
"Serial" CISCOs defaults: Speed (aka baud rate) = 9600 bits/s, 8 data bits, 1 stop bit, Parity (set to detect errors) = none, Flow control = none  

```
Router>
Router>enable
Router#
Router#configure terminal
```
`Router` - hostname of device  
`>` - user EXEC mode aka user mode - users can look but can't make any configuration changes
`enable` - enable `#`
`#` - priviledged EXEC mode - complete accsess to view dev. config, restart etc.
Can change time, save current conf. file BUT can't change the configuration
`configure terminal` - enters  `global configuration mode`
```
Router(config)#
Router(config)#enable passwo?
password
Router(config)#enable password ?
	7 Specifies a HIDDEN password will follow
	LINE The UNENCRYPTED (cleartext) 'enable' password
	level Set exec level password
Router(config)#enable password CCNA          (`enable secret` better)
Router(config)#exit
Router#exit
```
`?` if appended - shows full possible completions (usefull, bcos you can use only part of command if it's a unique part)
`?` if appended after a space - show all possible command next
`LINE` is all-uppercase bcos it's figurative, not literal
`exit` to return to privileged EXEC mode -> `exec` to logout

There are 2 separate conf. files on dev.:
`Running-config` - active conf. file. As you enter command you edit the active config. file
`Startup-config` - loaded on restart

`show running-config` - if it's the passwd-setting-in-cleartext-session it's in there!!!
`show startup-config`
`write` or `write memory` or `copy running-config startup-config` - save `running-config` as `startup-config` (without it passwd not saved)

For higher security:
`conf t` -> enter `global config mode` 
`service password-encryption` - weak encryption  (`enable secret` better)
OR

`enable secret CCNA` - MD5
`do sh run` - `do` enables priviledged EXEC mode from global conf. mode, `sh run` - shortcut of `show running-config`

`no service password-encrytpion` - `no` removes previously turned-on command  
Encrypted psswd is still encrypted in config file, but future changes will be in cleartext. (still - use only enable secret bcos MD5 is better than that)  

#### configure ip
```
R1>show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES unset  administratively down down
FastEthernet1/0            unassigned      YES unset  administratively down down
FastEthernet1/1            unassigned      YES unset  administratively down down
```
`show ip interface brief`:
- Interface - show interfaces (FastEthernet...);
- IP address - interface IP address;
- OK? - legacy from times when you coud assign invalid IP address;
- Method - by which interface was assigned an IP address;
- Status - layer 1 status. Is interface connected? 
	- `administratively down` - means it was disabled with `shutdown` command - default state of Cisco routers
	  BUT NOT Cisco switches -> interface would be up if connected to other device and down if not
- protocol - layer 2 status. If Status is down  (layer 1)-> Protocol (layer 2) must be down

`interface FastEthernet0/0` - configure that interface
`ip address 10.255.255.254 255.0.0.0` - assign IP address with that subnet mask
`no shutdown` - router interfaces are shutdown-disabled bydefault. BUT not switches
`do show ip interface brief` - check how are things now. 
```
R1>en
Password:
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#interface FastEthernet 0/0
R1(config-if)#ip address 10.255.255.254 ?
  A.B.C.D  IP subnet mask

R1(config-if)#ip address 10.255.255.254 255.0.0.0
R1(config-if)#no shutdown
R1(config-if)#
*Sep 26 18:37:14.203: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up      
                          ---> Layer 1 status change  <--------------------
*Sep 26 18:37:15.203: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up       ---> Layer 2 status change  <----------------
R1(config-if)#do show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            10.255.255.254  YES manual up                    up
FastEthernet1/0            unassigned      YES unset  administratively down down
```

`show interfaces fastEthernet 0/0` - maany info about particular interface (like MAC address)
```
R1#show interfaces fastEthernet 0/0
FastEthernet0/0 is up, line protocol is up ------------> Layer 1, Layer 2 status
  Hardware is DEC21140, address is ca01.0759.0000 (bia ca01.0759.0000) -> MAC address
  Internet address is 10.255.255.254/8   --------------> IP address
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Half-duplex, 100Mb/s, 100BaseTX/FX
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:00, output 00:00:02, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     918 packets input, 60554 bytes
     Received 974 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog
     0 input packets with dribble condition detected
     204 packets output, 21891 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     56 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier
     0 output buffer failures, 0 output buffers swapped out
```

`show interfaces description` - like `brief` but also has description (if someone configured one)
```
R1#show interfaces description
Interface                      Status         Protocol Description
Fa0/0                          up             up
Fa1/0                          up             up
Fa1/1                          admin down     down
```

`speed 100` max. speed on that interface
`duplex full` - always a good option if hubs are not used

#### configure description
`interfaces range f1/0 - 1` 
`description SOMETHING` - in interface configuration
```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#interface FastEthernet 0/0
R1(config-if)#description ## to Switch1 ##
R1(config-if)#int Fa1/0
R1(config-if)#desc ## to SW 2 ##
R1(config-if)#do show interface description
Interface                      Status         Protocol Description
Fa0/0                          up             up       ## to Switch1 ##
Fa1/0                          up             up       ## to SW 2 ##
Fa1/1                          admin down     down
```

`do show int status` - NOT working on the routers
```
SW1(config-if-range)#do show int status

Port      Name               Status       Vlan       Duplex  Speed Type
Et0/0     ## to SW2 ##       connected    1            auto   auto unknown
Et0/1     ## to end hosts ## connected    1            auto   auto unknown
Et1/3     ## not used ##     disabled     1            auto   auto unknown
```
	Name - description
	Status - connected / notconnect  (switch default startup-config make all of them seem connected).
	Switch interfaces are NOT shutdown on default

to check changes:
`do show running-config`
`do show interfaces description`
