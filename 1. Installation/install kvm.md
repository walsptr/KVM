# Installation


### install on ubuntu/debian
```
apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils -y
```
### install on redhat
```
dnf install qemu-kvm libvirt virt-install virt-viewer -y
```


### enable service

```
systemctl enable libvirtd
systemctl start libvirtd
systemctl status libvirtd
```

### Create VM
```
virt-install -n debian9-server --os-type=Linux --os-variant=debian9 --ram=1024 --vcpu=1 --network default --graphics none --location=/opt/debian-9.13.0-amd64-DVD-1.iso --extra-args console=ttyS0
```

### Connect VM using SSH
- Install libvirt-nss
```
dnf install libvirt-nss -y
```