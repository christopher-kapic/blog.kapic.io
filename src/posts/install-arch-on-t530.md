---
title: Arch Linux Installation Tutorial (Tested on T530)
date: 2021-08-27T08:05
thumb: pending.png
tags: 
    - Linux
    - Arch
    - Hardware
---

Follow along to install Arch Linux (tested on ThinkPad T530)

#### [Download the `.iso`](https://archlinux.org/download/)

#### Flash a USB
Use [Rufus](https://rufus.ie/) (Windows) or [Etcher](https://www.balena.io/etcher/) (MacOS or Linux) to flash the `.iso` to a flash drive.

#### Boot into the installation media
Common boot buttons include:
- `F12` - ThinkPads

#### Connect to the internet
Ethernet/virtual machine does not require any extra setup. For wifi, you will use the `iwctl` utility:

    iwctl device list
    iwctl station [device] get-networks
    iwctl station [device] connect [network]

After the last command runs, you will be prompted to enter a password if the wifi network is private.

_Note: If your network has multiple works, it must be enclosed in quotes._

You can test your connection by using the `ping` command (for example, `ping blog.kapic.io`).

#### Advanced: determine the boot mode

Check whether the `/sys/firmware/efi/efivars` directory exists:

    ls /sys/firmware/efi/efivars

If the directory exists, the computer boots with UEFI. If the directory does not exist, it probably boots with BIOS.

#### Optional: update the system clock

    timedatectl set-ntp true

#### Partition the drive

There are several options for partitioning the drive. You may use whichever you prefer; I suggest `cfdisk` because it is a GUI. ([Video reference](https://youtu.be/HpskN_jKyhc?t=774))

    cfdisk

The [Arch Linux Installation Guide](https://wiki.archlinux.org/title/Installation_guide) suggests using a root and a swap (> 512MiB) partition. If you are booting with UEFI, also create a boot partition.

#### Create a filesystem

Filesystem

    mkfs.ext4 /dev/[root partition]

Example

    mkfs.ext4 /dev/sda1

Swap

    mkswap /dev/[swap partition]

Example

    mkswap /dev/sda2

#### Mount the partitions

    mount /dev/[root partition] /mnt
    swapon /dev/[swap partition]

#### Install essential packages

    pacstrap /mnt base linux linux-firmware neovim

_Note: You can choose other text editors in place of neovim. If you do so, use that editor instead of `nvim` for the remainder of the tutorial._

#### Fstab

    genfstab -U /mnt >> /mnt/etc/fstab

#### Change root into the new system

    arch-chroot /mnt

#### Set the timezone

    ln -sf /usr/share/zoneinfo/[continent or country]/[city] /etc/localtime

Example

    ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime

_Note: to see available continents, run `ls /usr/share/zoneinfo/`. Likewise, to see available cities, run `ls /usr/share/zoneinfo/[continent]/`._

    hwclock --systohc

#### Choose locales

    nvim /etc/locale.gen

Uncomment the locales you wish to use (if you are not sure, just uncomment the line with `en_US.UTF-8 UTF-8`.

    locale-gen

Then run

    echo "LANG=[locale]" >> /etc/locale.conf

Example

    echo "LANG=en_US.UTF-8" >> /etc/locale.conf

#### Hostname

    echo [hostname] >> /etc/hostname

Edit `/etc/hosts`

    nvim /etc/hosts

```
# /etc/hosts
127.0.0.1          localhost
::1                localhost
127.0.1.1          [hostname].localdomain    [hostname]
```

#### Set the root password

    passwd

#### Install useful programs

To be honest, I am not sure what every one of these is for, but I tried installing Arch without them and ran into problems.

    pacman -S networkmanager network-manager-applet dialog wireless_tools wpa_supplicant os-prober mtools dosfstools base-devel linux-headers

#### Install a boot loader

Look up which boot loader is correct for the computer. I use `grub` for a ThinkPad T530.
__This step may be different depending on what hardward is being used. Do your own research to make sure you install a working boot loader.__

    pacman -S grub
    grub-install --target=i386-pc /dev/sda
    grub-mkconfig -o /boot/grub/grub.cfg

#### Exit and reboot

    exit
    
    umount -a
    
    reboot
