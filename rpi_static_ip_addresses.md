
# Static IP addresses on R PI



Edit `/etc/dhcpcd.conf`

```
interface eth0
static ip_address=192.168.1.10/24
static ip6_address=2001:470:6f:5ef::a:1/64
static routers=192.168.1.1
static domain_name_servers=192.168.1.1 8.8.8.8
```

Enable by the command `systemctl restart dhcpcd.service`

This configuration can hold just one IPv6 address.  
When we want set statically more IPv6 addresses, and we really want it.  
Then there is need to create file `/etc/systemd/network/20-ethernet.network`

Make content of this file:

```
[Match]
Name=eth0

[Network]
Address=2001:470:6f:5ef::a:2/64
Address=2001:470:6f:5ef::a:3/64
```

Enable by the command `systemctl enable --now systemd-networkd`

Sources: [link1](https://pimylifeup.com/raspberry-pi-static-ip-address/),
[link2](https://forums.raspberrypi.com/viewtopic.php?f=66&t=140252),
[link3](https://sleeplessbeastie.eu/2022/06/24/how-to-configure-multiple-ip-addresses-on-raspberry-pi-device/)
