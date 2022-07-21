Why is `recv. window` field so small [16 bits] when `seq. nr. field` is [32 bits]?
I get `recv. window` <= 1/2 `seq. nr. field`, but thats a huge difference

To add `wireshark` to `$PATH`:
- Windows:
	- Control Panel -> System -> Advanced -> Environment variables ...
	  You have to reboot to see changes


What if I spoof `RST`? (to try to disconnect someone)


Could I throttle everyones TCP on LAN spoofing multiple `duplicate ACK`s? (TCP Tahoe, TCP Reno)
(wireguard use UDP, OpenVPN can use UDP or TCP. I checked with TCP and it seems like it could be attacked like this)

in `slow start`:
set `ssthresh` = `cwnd`/2 && set `cwnd` = 1 -> `slow start`

in `congestion avoidance`:
if 3x `duplicate ACK` (less worrying than `timeout` bcos `ACK`s delivered)-> set `sstresh` = `cwnd`/2 && set `cwnd` = `sstresh` + 3`MSS` -> `fast recovery`

in `fast recovery`:
increase `cwnd` by 1 `MSS` every `duplicate ACK`  received for missing segment that caused TCP to enter `fast recovery`
BUT
It is nonetheless decreased (`cwnd` = `sstresh` + 3`MSS`) in `congestion avoidance` !!!