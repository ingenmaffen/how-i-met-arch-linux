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

```shell
cfdisk /dev/sda
```

dos
new, primary, bootable, 100M
new, primary, 223.5G
save, quit

```shell
cryptsetup luksFormat --type luks1 --use-random -S 1 -s 512 -h sha512 -i 5000 /dev/sda2
cryptsetup open --type luks /dev/sda2 cryptroot
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/mapper/cryptroot
mount -t ext4 /dev/mapper/cryptroot /mnt
mkdir -p /mnt/boot
mount -t ext4 /dev/sda1 /mnt/boot
```

```shell
pacstrap -i /mnt base base-devel linux linux-firmware nano mkinitcpio dhcpcd wpa_supplicant
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Europe/Budapest /etc/localtime
hwclock --systohc
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
echo "127.0.0.1 localhost" > /etc/hostname
```

```shell
passwd
useradd -m -g users username
passwd username
```

```shell
pacman -S grub-bios os-prober
nano /etc/default/grub
```

```shell
GRUB_CMDLINE_LINUX="cryptdevice=/dev/sda2:cryptroot"
```

```shell
nano /etc/mkinitcpio.conf
```

```shell
... block encrypt filesystem ...
```

```shell
mkinitcpio -p linux
grub-install --recheck /dev/sda
grub-mkconfig --output /boot/grub/grub.cfg
exit
```

```shell
umount -R /mnt/boot
umount -R /mnt
cryptsetup close cryptroot
systemctl reboot
```

```shell
ip link
wpa_supplicant -B -i wlp2s0 -c<(wpa_passphrase ssid password)
dhcpcd
pacman -S xorg xorg-server plasma sddm konsole
pacman -S networkmanager network-manager-applet
systemctl enable NetworkManager
systemctl enable sddm
systemctl start sddm
```

```shell
su
nano /etc/sudoers
```

```shell
root ALL=(ALL) ALL
username ALL=(ALL) ALL
```
