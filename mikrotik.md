
# Mikrotik
model: MikroTik RouterBOARD RB760iGS, hEX S


## How to create IPv6 tunnel

Sources:  
https://help.mikrotik.com/docs/display/ROS/6to4  
https://www.tunnelbroker.net/

Registration on website tunnelbroker.net required.  
Login and ask about tunnel. Insert public IPv4 address and IPv6 settings will be generated for you.

```
 # interface 6to4 add disabled=no local-address=194.105.56.170 mtu=1280 name=6to4-tunnel1 remote-address=216.66.80.90
 # ipv6 route add disabled=no distance=1 dst-address=2000::/3 gateway=2001:470:6e:5ee::1 scope=30 target-scope=10
 # ipv6 address add address=2001:470:6e:5ee::2/64 advertise=no disabled=no eui-64=no interface=6to4-tunnel1
 # ipv6 address add address=2001:470:6f:5ee:: interface=bridge advertise=yes
 # ipv6 nd set [ find default=yes ] advertise-dns=yes
 # ip dns set allow-remote-requests=yes servers=2001:4860:4860::8888,2001:4860:4860::8844
```
> **_NOTE:_**  4th command contains address with **6f** instead of **6e**. It is because 6e is for tunnel itself. For client usage is 6f address space.

## Switch WAN port to ether2 (from default ether1)

I case you want to use POE with your LAN switch, you need to change WAN to port2 and make from port1(support POE) normal LAN port. 
Then you can connect LAN switch with port1(PoE) and ISP cabel(source of internet) to port2(now WAN port).

Tricky part is go to Bridge config and change port2 to port1. Then port2 is not "secondary" and can be used for DHCP client.

Source: https://forum.mikrotik.com/viewtopic.php?t=162832
