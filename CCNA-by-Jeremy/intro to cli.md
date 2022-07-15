may be a physical port (e.g. `RJ-45`, `USB mini B`)

`RJ45` use `Rollovercable`  
pins pairs:  1-8,  2-7,  3-6,  4-5,  5-4,  6-3,  7-2,  8-1

You need terminal emulator - `PuTTy` is a popular choice. If connected physically use "Serial" option  
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
`exit` to return to privilidged EXEC mode -> `exec` to logout

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

