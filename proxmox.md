
# Proxmox

## Installation on too much new hardware (GPU not detected)

Source: https://forum.proxmox.com/threads/generic-solution-when-install-gets-framebuffer-mode-fails.111577/

`cd /usr/share/X11/xorg.conf.d/`  
`touch radeon.conf`  
`vi radeon.conf`

file content:
```
Section "Device"
    Identifier "Card0"
    Driver "fbdev"
    BusID "0b:00.0"
EndSection
```

`xinit -- -dpi 96 >/dev/tty2 2>&1`


## Install client agent on guest machine

`apt install qemu-guest-agent`
