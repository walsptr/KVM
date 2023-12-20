# Bridging using OpenVswitch

## Installation
```
subscription-manager repos --enable=openstack-16-for-rhel-8-x86_64-rpms
subscription-manager repos --enable=fast-datapath-for-rhel-8-x86_64-rpms
yum install https://rdoproject.org/repos/rdo-release.rpm
yum install openvswitch3.1 libibverbs

```
enable openvswitch
```
systemctl enable --now openvswitch
```



## Konfigurasi

membuat network bridge

```
ovs-vsctl add-br ovs-br
ifconfig ovs-br up
```

menambahkan port bridge
<br>note: setelah menambahkan port, nantinya kalian akan kehilangan koneksi dari port internet (enp0s3)
```
ovs-vsctl add-port ovs-br enp0s3
```

hidup dan matikan interface internet, agar dhcp client di arahkan ke port bridge ovs
```
nmcli con down enp0s3
nmcli con up enp0s3
```

check dhcp client
```
dhclient ovs-br
```

selanjutnya bisa dicoba langsung dimasukin ke vm yang akan dibuat
```
virt-install -n debian9-server --os-type=Linux --os-variant=debian9 --ram=1024 --vcpu=1 --network=bridge:ovs-br,virtualport_type=openvswitch --graphics none --location=/opt/debian-9.13.0-amd64-DVD-1.iso --extra-args console=ttyS0
```

<!-- ```
ip tuntap add mode tap vport1
ifconfig vport1 up
ovs-vsctl add-port ovs-br vport1
ovs-vsctl show
```

Buat sebuah file xml untuk define vport baru ke libvirt, untuk ip address yang digunakan pastikan satu network dengan interface ovs-br
```
nano /tmp/vport1.xml
```
isi file
```
<network>
  <name>vport1</name>
  <forward mode='nat'/>
  <bridge name='vport1' stp='on' delay='0'/>
  <ip address='192.168.189.100' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.189.200' end='192.168.189.254'/>
    </dhcp>
  </ip>
</network>
```

```
virsh net-define /tmp/vport1.xml
virsh net-list --all
virsh net-start vport1
```
Selanjutnya kalian bisa langsung mendefine network openvswitch ke kvm kalian -->