
# Linux (Ubuntu)

following logs for specific service  
`journalctl -u service_name -f`

in case ping is not found:  
`apt install iputils-ping`

in case ifconfig is not found  
`apt install net-tools`

install agent to KVM VM `apt install qemu-guest-agent`

reload systemd `systemctl daemon-reload`

set timezone `dpkg-reconfigure tzdata`
