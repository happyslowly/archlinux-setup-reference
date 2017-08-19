# Installation Guide

## Wireless

```bash
ip link set <interface> up

iw dev <interface> scan
wpa_supplicant -B -i <interface> -c <(wpa_passphrase "your_SSID" "your_key")
dhcpcd <interface>

# test
ping www.google.com
```

## Clock

```bash
timedatectl set-ntp true
```

## Partition & Filesystem

```bash
fdisk -l
cfdisk

mkfs.ext4 /dev/sda3
mkfs.ext4 /dev/nvme0n1p1
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2

mount /dev/sda3 /mnt
mkdir -p /mnt/boot /mnt/home
mount /dev/sda1 /mnt/boot
mount /dev/nvme0n1p1 /mnt/home
```

## Install Base

```bash
pacstrap /mnt base base-devel
```

## Configure System

```bash
genfstab -U /mnt > /mnt/etc/fstab

arch-chroot /mnt
ln -sf /usr/share/zoneinfo/US/Pacific /etc/localtime
hwclock --systohc

vi /etc/local.gen
locale-gen
vi /etc/locale.conf

vi /etc/hostname

pacman -S iw wpa_supplicant

passwd

pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
pacman -S intel-ucode
grub-mkconfig -o /boot/grub/grub.cfg
```

## Cleanup

```bash
exit
umount -R /mnt
reboot
```

## X Window

```bash
pacman -S xf86-video-intel mesa
pacman -S xorg-xinit
pacman -S gnome
```