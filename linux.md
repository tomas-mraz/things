
# Ubuntu (Linux)

## System tweaks

following logs for specific service  
`journalctl -u service_name -f`

in case ping is not found:  
`apt install iputils-ping`

in case ifconfig is not found  
`apt install net-tools`

install agent to KVM VM `apt install qemu-guest-agent`

reload systemd `systemctl daemon-reload`

set timezone `dpkg-reconfigure tzdata`

## How to add application icon to system

Source: https://askubuntu.com/questions/1428517/how-do-i-create-a-new-application-launcher-in-ubuntu-22-04

- create file `~/.local/share/applications/goland.desktop` with content
```
[Desktop Entry]
Name=GoLand
Comment=xxx
Type=Application
Exec=/opt/GoLand-2025.1.1/bin/goland
Icon=/opt/GoLand-2025.1.1/bin/goland.png
```
- some apps specially Java based created new icon after lunch and do not pair with original icon
    - to fix that run application
    - run in terminal `xprop | grep WM_CLASS` and click on window of application by special cursor
    - in terminal, you get "window class"
    - add to .desktop file row with `StartupWMClass=value-of-window-class`
- icon shows up automatically in main menu (bottom-left icon)
- in main menu right-click on icon and "Pin to Dash"
