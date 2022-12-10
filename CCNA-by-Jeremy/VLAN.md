`VLAN` (virtual LAN) - switch WON't forward traffic between VLANs (including **broadcast&unknown unicast**)
	- you can use one switch for many subnets (broadcast won't be flooded into other subnets if in other VLANs)
	- better performance (less broadcast than needed), better security
- switch **doesn't perform** inter-VLAN routing
- configured per interface

`show vlan brief`
```

```
- by default - all interfaces in VLAN 1
- VLANs 1, 1002-1005 cannot be deleted

switchport - interface of a switch (I belive)
access port - switchport which belong to single VLAN, usually connects to end-hosts
trunk port - switchport which carry multiple VLANs

`switchport mode access` - set switchport as access port
should happen by default, but it's a good practice to do so manually

`switchport access vlan 10` - assign VLAN to a port
if VLAN doesn't exist it is created

`vlan 10` - configuration of VLAN 10
if VLAN doesn't exist it is created
`name SOMETHING`
