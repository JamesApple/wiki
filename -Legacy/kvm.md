```sh
# Virtualization Packages
sudo dnf -y install \
    bridge-utils \
    libvirt \
    virt-install \
    qemu-kvm


# Enable KVM Daemon libvirtd
sudo systemctl start libvirtd
sudo systemctl enable libvirtd

# Virtualization Helpers
# https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/
# https://computingforgeeks.com/how-to-mount-vm-virtual-disk-on-kvm-hypervisor/
sudo dnf -y install \
    virt-top \
    libguestfs-tools \
    virt-manager

virt-install \
--name fed29 \
--ram 1024 \
--vcpus 1 \
--disk path=/var/lib/libvirt/images/fed29.img,size=20 \
--os-variant fedora29 \
--os-type linux \
--network bridge=br0 \
--graphics none \
--console pty,target_type=serial \
--location 'http://fedora.inode.at/releases/29/Server/x86_64/os/' \
--extra-args 'console=ttyS0,115200n8 serial'

# https://computingforgeeks.com/virsh-commands-cheatsheet/
virsh console fed29
```
