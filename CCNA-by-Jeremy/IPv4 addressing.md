127.0.0.0 - 127.255.255.255 address range for testing "network stack" on local device

ip address class - first octet - mask
- A - 0xxxxxxx - /8
- B - 10xxxxxx - /16
- C - 110xxxxx - /24

If `host portion` of IP address = all 0's -> Network ID
If `host portion` of IP address = all 1's -> Broadcast ID

So if dst. IP is e.g. 192.168.1.255 (on /24 network) -> frame dst. MAC is FFFF.FFFF.FFFF

