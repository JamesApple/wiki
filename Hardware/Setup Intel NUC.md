# Setup Intel NUC

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




