# Things
... to remember

- K9s is a Kubernetes tool which is the must have [K9s homepage](https://k9scli.io)
- How to make my own GitLab runner with IPv6 [[page](./gitlab-runner.md)]
- Create 6to4 tunnel on Mikrotik router [[page](mikrotik.md#how-to-create-ipv6-tunnel)]
- Change WAN to ether2 and use ether1 with PoE to connect LAN on Mikrotik router [[page](mikrotik.md#switch-wan-port-to-ether2-from-default-ether1)]
- IPv4 SSL to IPv6 no-SSL reverse proxy with Nginx [[page](nginx_ssl_proxy.md)]
- Ubuntu Netplan IPv4 and IPv6 settings [[page](./netplan.md)]
- Signing commits - "Verified" label on GitHub [[page](./signing_commits.md)]

## Docs
- [Mermaid](https://mermaid-js.github.io/mermaid/) - Graphs in markdown
- https://stackedit.io - Markdown online editor
- https://github.com/asciidoctor/asciidoctor

## Code
- API Designer [Swagger/OpenAPI](https://swagger.io/)  

## Networking 

- Raspberry PI [manual setting IPv4 and IPv6 address](./rpi_static_ip_addresses.md)

## Grafana

- Prometheus exporter for Raspberry Pi https://github.com/lukasmalkmus/rpi_exporter

## Red Hat firewall
- firewall-cmd --zone=public --add-port=8484/tcp --permanent
- firewall-cmd --reload
- systemctl status firewalld

## Login into Tanzu
kubectl vsphere login --server=hostname.com --vsphere-username user.me@tanzu --tanzu-kubernetes-cluster-namespace iot-test --tanzu-kubernetes-cluster-name iot-test-tkc01 --insecure-skip-tls-verify
