### configuring network bonding

```
$ nmcli connection show
$ nmcli dev status
$ nmcli connection add type bond con-name bond0 ifname bond0 bond.options
"mode=active-backup"
$ nmcli con add type ethernet slave-type bond con-name bond0:1 ifname ens65f0
master bond0
$ nmcli con add type ethernet slave-type bond con-name bond0:2 ifname ens67f1
master bond0
$ nmcli con mod bond0 ipv4.addresses "10.76.33.94/24" <<<<< Put IP address
which should be resolve on DNS.
$ nmcli con mod bond0 ipv4.gateway 10.76.33.1
$ nmcli con mod bond0 ipv4.dns "10.33.32.111"
$ nmcli con mod bond0 ipv4.method manual
$ nmcli con up bond0
```
### configuring NTP

```
$ dnf install -y chrony
$ systemctl enable --now chronyd
```
### configuring 

```
$ systemctl enable --now firewalld
$ firewall-cmd --permanent --add-service=ssh
$ firewall-cmd --reload
```

```
$ dnf config-manager --enable ol8_baseos_latest ol8_appstream
$ dnf install -y oracle-ovirt-release-45-el8
$ dnf clean all
$ dnf repolist
```

```
$ dnf install -y kernel-uek-modules-extra
$ dnf update -y
$ reboot
$ uname -r
```

```
$ dnf groupinstall -y "Virtualization Host"
$ systemctl enable --now libvirtd
```