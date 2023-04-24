
# Mikrotik 

MikroTik RouterBOARD RB760iGS, hEX S

I case you want to use POE with your LAN switch, you need to change WAN to port2 and make from port1(support POE) normal LAN port. 
Then you can connect LAN switch with port1(PoE) and ISP cabel(source of internet) to port2(now WAN port).

Tricky part is go to Bridge config and change port2 to port1. Then port2 is not "secondary" and can be used for DHCP client.

Sources:
- https://forum.mikrotik.com/viewtopic.php?t=162832

