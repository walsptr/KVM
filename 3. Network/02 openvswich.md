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
virt-install --memory 2048 --vcpus 2 --name mycentos --disk /var/lib/libvirt/images/CentOS-8-ec2-8.3.2011-20201204.2.x86_64.qcow2,device=disk,bus=virtio,format=qcow2 --cloud-init user-data=user-data --os-type Linux --os-variant centos8 --network bridge:ovs-br,virtualport_type=openvswitch --graphics none --import
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