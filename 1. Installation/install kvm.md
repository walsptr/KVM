# Installtion

```
apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils -y
```

### enable service

```
systemctl enable libvirtd
systemctl start libvirtd
systemctl status libvirtd
```