# How I Met Arch Linux

## The purpose of all this

I started installing Arch Linux 2 days ago and I fucked up everything that was possible to fuck up, so I want to document all the good things I did while installing Arch Linux.

These are the guides I used during the process:

[ArchWiki Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide)
[Some random article about disk encryption](https://www.howtoforge.com/tutorial/how-to-install-arch-linux-with-full-disk-encryption/)

[ArchWiki WPA supplicant documentation (for wireless connection)](https://wiki.archlinux.org/index.php/WPA_supplicant)

## Notes

I install Arch Linux on a notebook so I use wireless connection, so if you (or I) use wired connection, these steps may differ

## Pre-insall stuff

First of all (after making a bootable USB drive with Arch Linux on it and boot up Arch Linux installer) set your keyboard with

```shell
loadkeys hu
```

then set up your wireless connection using WPA supplicant. List your interfaces with 

```shell
ip link
```

and if it list more than one option, you should wildly guess which one to use; I got lo, eno1, wwp0s29u1u6i6 and wlan0 and (miraculously) wlan0 is the one I will use. Write 

```shell
ctrl_interface=/run/wpa_supplicant
update_config=1
```

into /etc/wpa_supplicant/wpa_supplicant.conf with

```shell
echo "ctrl_interface=/run/wpa_supplicant\nupdate_config=1" > /etc/wpa_supplicant/wpa_supplicant.conf
```

and after this you can run

```shell
wpa_supplicant -B -i <interface> -c /etc/wpa_supplicant/wpa_supplicant.conf
```
where -B helps run it in the background and you should write your interface at <interface> like this

```shell
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```

and if everything is awesome, you can run 

```shell
wpa_cli
``` 

where you can do a bunch of cool shit, like 

```shell
add_network
set_network 0 ssid "MYSSID"
set_network 0 psk "passphrase"
enable_network 0
``` 

and after doing so, you should save your setting and leave wpa_cli with

```shell
save_config
quit
``` 

and run

```shell
dhcpcd
``` 

to start your networking service (I guess). If you made this far, you can run 

```shell
ping archlinux.org
``` 

and hopefully you will get some response. After all this, set your clock with

```shell
timedatectl set-ntp true
``` 
but to be honest, I don't know what it actually does, but it was in the install guide and I'm not taking any chances, so you should run it too.

After all this, fuck up your current file system. Get all the information you with 

```shell
fdisk -l
``` 
but you probably not gonna use it, because if you want to encrypt your disk, you will run

```shell
shred --verbose --random-source=/dev/urandom --iterations=3 /dev/sda
``` 
which takes about 4 hours on a 256GB SSD and it completely fucks up your partitions, which is good, I guess.