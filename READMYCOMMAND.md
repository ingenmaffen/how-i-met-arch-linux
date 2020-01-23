# How I Met Arch Linux - TL;DR Edition

```shell
loadkeys hu
echo "ctrl_interface=/run/wpa_supplicant\nupdate_config=1" > /etc/wpa_supplicant/wpa_supplicant.conf
ip link
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
wpa_cli
```

```shell
add_network
set_network 0 ssid "MYSSID"
set_network 0 psk "passphrase"
enable_network 0
save_config
quit
```  

```shell
dhcpcd
ping archlinux.org
timedatectl set-ntp true
shred --verbose --random-source=/dev/urandom --iterations=3 /dev/sda
``` 
