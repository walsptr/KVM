# oVirt Nodes

### Minimum Requirements
- Using Centos / RedHat
- RAM: 8GB
- Disk: 64 GB

### Checking CPU for enablef Virtualization

```
egrep -c '(vmx|svm)' /proc/cpuinfo
```
if the result more than 0 it's good, of not u must turn on CPU virtualization

### Installation oVirt

```
dnf -y install https://resources.ovirt.org/pub/yum-repo/ovirt-release44.rpm
dnf module -y enable javapackages-tools
dnf module -y enable pki-deps
dnf module -y enable postgresql:12
dnf module -y enable mod_auth_openidc:2.3
dnf module -y enable nodejs:14
dnf distro-sync --nobest
dnf upgrade --nobest
dnf install ovirt-engine -y
engine-setup
```

### Create SSL

- Checking where directory SSL default
```
ovirt-imageio --show-config | jq .tls
```

- Create CA
```
cd /etc/pki/ovirt-engine/certs

```

- Create Certi SSL
```

```