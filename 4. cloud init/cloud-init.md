# Create VM using Cloud init

### Installation
```
dnf install cloud-init -y
```
Enable service cloud init
```
systemctl enable --now cloud-init
```

### Configuration
- Download DVD
```
cd /var/lib/libvirt/images
wget https://cloud.centos.org/centos/8/x86_64/images/CentOS-8-ec2-8.3.2011-20201204.2.x86_64.qcow2
```

Buat terlebih dahulu direktori untuk menyimpan file userdata
```
mkdir cloudinit
cd cloudinit
```

- Create ssh key
```
ssh-keygen
```
copy file pubkey ke direktori cloudinit
```
cp ~/.ssh/id_rsa.pub /var/lib/libvirt/images/cloudinit/user-data
```

- Create user-data
```
vim user-data

---isi file---
#cloud-config
system_info:
  default_user:
    name: user
    home: /home/technekey
    sudo: ALL=(ALL) NOPASSWD:ALL
password: password
chpasswd: { expire: False }
ssh_pwauth: True
ssh_authorized_keys:
- ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCyzPxfmOeOeZfGNKpw04OcR8kQcOeSc1zlCZjCf8dPY39TEdng2APMIjmISbQ/I7phuNryH0LU+1Iwnu7R3UQBYUq0/1V8ciKE7emPtPATdI/OBMxVFzy89RST+IHpevorp6BEF8P7v/XaEXCTuin/eQUESsc51nIGKbOcMBMWLj6FTV30iLvEnRy8xwxIR3jBCF4vHj9fv1YC7tzn2KnpahfRzOe7QaGh0SEtMz5Gg3L91ytQLNJrjSkRhSwa1xZHSbAqVDMZGK8jVNLdWJZZk9vYDk705qmtwCuaH4kyTVNB1BEjQG3nnCdjcArC2DANAUUm2dbz6dT3wCBVE2RhOZAi74vXAQ8vvlp5yitcgvD0BvniIomdQm8dI/JiesaVkYMklJCmvieOn8XhZolQp3JKDcnOSVvmDPMHsxSm12cfynscq5CODuZ8cQRHsqqxED1ZUXodfdmi1JTk5FEzBYha49qAxw8UJ7OnwUGAzXzkJ6231tDuAO7zxwZPdvU= root@localhost.localdomain
---end---
```

- Create VM with cloud init
```

```