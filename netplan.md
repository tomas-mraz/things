
Network configuration for one static IPv4 and automatic setup IPv6 + few static IPv6.  
It is not need to care about default route.

```
network:
  ethernets:
    ens18:
      dhcp4: false
      dhcp6: false
      accept-ra: true
      addresses:
        - 192.168.10.201/24
        - 2001:470:6f:5ef::c:1/64
        - 2001:470:6f:5ef::c:2/64
      nameservers:
        addresses:
          - 192.168.10.1
          - 1.1.1.1
          - 8.8.8.8
          - 2606:4700:4700::1111
          - 2001:4860:4860::8888
      routes:
        - to: default
          via: 192.168.10.1
          on-link: true
  version: 2
```
