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