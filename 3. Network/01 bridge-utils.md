# Create Network 

### Enable ipv4 forward

```
vim /etc/sysctl.conf

hapus tanda pagar pada baris 
net.ipv4.ip_forward=1
```

### Create new file network 01-netcfg.yaml
```
vim /etc/netplan/01-netcfg.yaml
```

```
network:
  ethernets:
    ens160:
      dhcp4: false
      dhcp6: false
  # add configuration for bridge interface
  bridges:
    br0:
      interfaces: [ens160]
      dhcp4: false
      addresses: [172.23.3.200/20]
      macaddress: 00:50:56:89:1f:47
      routes:
        - to: default
          via: 172.23.0.1
      nameservers:
        addresses: [8.8.8.8]
      parameters:
        stp: false
      dhcp6: false
  version: 2
```

### define bridge network

```
vim br0.xml
```

```
<network>
  <name>br0</name>
  <forward mode="bridge"/>
  <bridge name="br0"/>
</network>
```

### start bridge network
```
virsh net-define /path/br0.xml
virsh net-start br0
virsh net-autostart br0
```

### list network
```
virsh net-list
```

### Create VM 
```
virt-install --memory 2048 --vcpus 2 --name mycentos --disk /var/lib/libvirt/images/CentOS-8-ec2-8.3.2011-20201204.2.x86_64.qcow2,device=disk,bus=virtio,format=qcow2 --cloud-init user-data=user-data --os-type Linux --os-variant centos8 --network br0 --graphics none --import
```