
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
Then you can connect LAN switch with port1(PoE) and ISP cable (source of internet) to port2 (now WAN port).

Tricky part is go to Bridge config and change port2 to port1. Then port2 is not "secondary" and can be used for DHCP client.

Source: https://forum.mikrotik.com/viewtopic.php?t=162832

## Hex PoE warm up
Common:
- disable packages "wireless", "ppp" and "hotspot" in /system/package
- enable package "ipv6" in /system/package
- disable spf1 in /interfaces/interface
- disable ftp,ssh,telnet in /ip/services
- in /system/clock set time-zone-name=Europe/Prague
- in /system/sntp client set enabled=yes primary-ntp=195.113.144.201 secondary-ntp=147.32.127.250
- in /tools/graphing/interface-rules add interface=ether2 store-on-disk=yes
- in /tools/graphing/resource-rules add rule store-on-disk=yes
- in /ip/dns set servers: 1.1.1.1, 8.8.8.8

Specific to switch eth1 with eth2
- change interface ether2 to ether1 in /bridge/ports
- change WAN from ether1 to ether2 in /interfaces/interface-list
- change interface ether1 to ether2 in /ip/dhcp-client
- in /ip/dhcp-client set enable=yes interface=ether2
- in /ipv6/dhcp-client set request=prefix interface=ether2 pool-name=pool6 pool-prefix-length=48 use-peer-dns=no rapid-commit=no add-default-route=yes

???
- in /ip/neighbor-discovery set discover-interface-list=LAN

- upload file config_firewall_ipv4.rsc
- upload file config_firewall_ipv6.rsc
- terminal /import file-name=config_firewall_ipv4.rsc
- terminal /import file-name=config_firewall_ipv6.rsc
