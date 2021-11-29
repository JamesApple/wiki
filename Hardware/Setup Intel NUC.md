# Setup Intel NUC

https://sylvaindurand.org/remotely-unlock-an-encrypted-system/

Installing Archlinux on an Intel i3 NUC

[Official Archlinux Install Guide](https://wiki.archlinux.org/title/Installation_guide)

## Update BIOS

[Search and download a .cap for the latest BIOS on the intel downloads page](https://www.intel.com/content/www/us/en/download-center/home.html) using the device code e.g. `NUC10i5FNHJ`. Ensure that 

Write the bios update to a thumb drive (/dev/sda)

```sh
sudo mkfs.fat /dev/sda
sudo mount /dev/sda /mnt
sudo cp ~/Downloads/<BIOS FILE>.cap /mnt
sudo umount /mnt
```

Restart the NUC with the BIOS drive plugged in and press F7. Select the .cap file from your drive and wait.

Write the Arch ISO to a bootable thumb drive
```sh
# Work it out silly
```
Disable secure boot and boot from the ISO

Use `cfdisk` for a TUI 

Follow the [UEFI setup table](https://wiki.archlinux.org/title/Partitioning#UEFI/GPT_layout_example) but ignore the swap.

```

# Drive labeled as gpt
Device            Start        End   Sectors   Size Type
/dev/nvme0n1p1     2048    1050623   1048576   512M EFI System
/dev/nvme0n1p2 42993664 1000215182 957221519 456.4G Linux filesystem
```


Setup FDE and mount the root partition `/dev/nvme0n1p2`

```sh
cryptsetup luksFormat /dev/nvme0n1p2
cryptsetup open /dev/nvme0n1p2 cryptroot
mkfs.ext4 /dev/mapper/cryptroot
mount /dev/mapper/cryptroot /mnt
```

Format and mount UEFI boot filesystem
```sh
mkfs.fat -F32 /dev/nvme0n1p1
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```

Bootstrap system
```sh
pacstrap /mnt base linux linux-firmware neovim dhcpcd
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Australia/Sydney /etc/localtime
hwclock --systohc

# Uncomment en_US.UTF-8 in /etc/locale.gen
locale-gen
localectl set-locale LANG=en_US.UTF-8
```
Edit `/etc/mkinitcpio.conf` and add encrypt before `filesystems` and after `keyboard`

`HOOKS=(base udev autodetect modconf block keyboard encrypt filesystems fsck)`

Generate initramfs
`mkinitcpio -P`

Patch ucode
`pacman -S intel-ucode`

`bootctl install`

Create `/boot/loader/entries/arch.conf` and replace uuid XXX... with the uuid for the root partition `nvme0n1p2` from `blkid`
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /intel-ucode.img
initrd  /initramfs-linux.img
options cryptdevice=UUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX:cryptroot root=/dev/mapper/cryptroot rw
```

Set `/boot/loader/loader.conf`
```
default      arch.conf
timeout      5
console-mode max
editor       no
```

Preview config with `bootctl list`

`passwd`

`reboot`

After reboot install SSH server


Login with
`root` and the password set before

```sh
systemctl enable dhcpcd
pacman -S openssh

# Set `PermitRootLogin yes` in /etc/ssh/sshd_config
systemctl enable sshd.service
systemctl start sshd.service
```
