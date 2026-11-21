# Home Assistant

## using CM4-POE-UPS-BASE
USB2.0 is disabled by default, if you want to enable it, you need to add `dtoverlay=dwc2,dr_mode=host` in `config.txt`.  
It change USB to host mode.

### turn-off red LED
Add line `dtparam=pwr_led_trigger=none` into `config.txt` file.

## console CLI
reset password `auth reset --username john --password 12345`

turn-off HA computer
```shell
# core stop
# host shutdown
```
