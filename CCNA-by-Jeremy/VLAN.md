`VLAN` (virtual LAN) - switch WON't forward traffic between VLANs (including **broadcast&unknown unicast**)
	- you can use one switch for many subnets (broadcast won't be flooded into other subnets if in other VLANs)
	- better performance (less broadcast than needed), better security
- switch **doesn't perform** inter-VLAN routing
- configured per interface

`show vlan brief` - show only `access ports` assigned to VLANs
```
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et0/1, Et0/2, Et0/3
                                                Et1/0, Et1/1, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```
- by default - all interfaces in VLAN 1
- VLANs 1, 1002-1005 cannot be deleted

switchport - interface of a switch (I belive)
access port (aka untagged port) - switchport which belong to single VLAN, usually connects to end-hosts
trunk port (aka tagged port)- switchport which carry multiple VLANs

`switchport mode access` - set switchport as access port
should happen by default, but it's a good practice to do so manually

`switchport access vlan 10` - assign VLAN to a port
if VLAN doesn't exist it is created

`vlan 10` - configuration of VLAN 10
if VLAN doesn't exist it is created
`name SOMETHING`


### trunkning protocols
for inter-VLAN switching using `trunk port`s

#### ISL (Inter-switch link)
proprietary CISCO protocol (older then dot1q) -> not supported even on newer CISCO dev.

#### IEEE 802.1Q (aka dot1q)
`802.1Q tag` - new `eth. frame` header field between `src. MAC` & `type` fields [4bytes = 32bits]
	- `TPID` (tag protocol identifier) [2bytes = 16bits] - indicate that frame is 802.1Q-tagged
	  always 0x8100
	- `TCI` (tag control information) [2bytes = 16bits]
		- PCP (priority code point) [3bits] - for CoS (Class of Service) - important traffic prioritization in congested network
		- DEI (drop eligible indicator) [1bit] - indicate frames that can be dropped if congested
		- VID (VLAN ID) [12bits] - identifies to which VLAN `eth. frame` should go from `trunk port`
		  VLANs 0 & 4095 are reserved (can't be used)
		  normal VLANs: 1-1005
		  extended VLANs: 1006-4094

`native VLAN` - all frames without `802.1Q tag` are sent to preconfigured "native" VLAN
	- by default `native VLAN` == `VLAN 1` on all trunk ports
	- you have to configure **EACH** `trunk port` to use particular VLAN
		be carefull for mismatch of `native VLANs` on to connected `trunk port` interrfaces
			EACH connected `trunk port` has to understand `native VLANs` in the same way - frame with `802.1Q tag` meant for `VLAN 30` won't be forwarded if `VLAN 30` is a `native VLAN` !!!!!!!


`switchport trunk encapsulation dot1q` - if switch supports `ISL` (is old) you have to set encapsulation to `802.1Q` before. On newer it's unneccesary

`switchport mode trunk` - set switchport as trunk port

`show interfaces trunk`
```
 
```
	 Mode: on - manually configured as a trunk
	 Encapsualtion: `dot1Q` or `ISL`
	VLANS allowed on ttunk - SECURITY&performance - disable not needed

`switchport trunk allowed vlan ?`
```

```
	e.g.
	`switchport trunk allowed vlan 10,30`
	`switchport trunk allowed vlan add 20` - add 20 to list of allowed
	`switchport trunk allowed vlan remove 20`
	`switchport trunk allowed vlan all` - default state
	`switchport trunk allowed vlan except 1-5,10`
	`switchport trunk allowed vlan none`

**It's good to change `native VLAN` to unsued VLAN**

`switchport trunk native vlan 1001` - change native VLAN to 1001

### Router on a stick (ROAS)
(one cable looks like a stock on network diagram)

route multiple VLANs using one interface onthe router & switch
router physical interface divided into many logical subinterfaces (for each VLAN)

configure ROAS on router (switch is connected with it's `trunk port`)

To make subinterface for g0/0:
`interface g0/0.10` - 0.10 logical subinterface
```
encapsulation dot1q 10
ip address 192.168.1.1 255.255.255.128
int g0/0.20
encapsulation dot1q 20
ip address 192.168.1.128 255.255.255.128
```
...
VLANs subinterface nr. doesn't have to match VLAN nr. (but it's a good practice)
when frame sent from subinteface it will have `802.1Q tag` for configured VLAN
