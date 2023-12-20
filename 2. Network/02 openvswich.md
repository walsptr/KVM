# Bridging using OpenVswitch

## Installation
```
subscription-manager repos --enable=openstack-16-for-rhel-8-x86_64-rpms
subscription-manager repos --enable=fast-datapath-for-rhel-8-x86_64-rpms
yum install https://rdoproject.org/repos/rdo-release.rpm
yum install openvswitch libibverbs

```
enable openvswitch
```
systemctl enable --now openvswitch
```



## Konfigurasi

membuat network bridge

```
ovs-vsctl add-br ovs-br
nmcli con up ovs-br
```

menambahkan port bridge
note: setelah menambahkan port, nantinya kalian akan kehilangan koneksi dari port internet (enp0s3)
```
ovs-vsctl add-port ovs-br enp0s3
```

hidup dan matikan interface internet, agar dhcp client di arahkan ke port birdge ovs
```
nmcli con down enp0s3
nmcli con up enp0s3
```

check dhcp client
```
dhclient ovs-br
```

Selanjutnya kalian bisa langsung mendefine network openvswitch ke kvm kalian