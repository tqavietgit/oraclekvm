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

### configuring iSCSI

```
$ sudo dnf install -y iscsi-initiator-utils
$ WWN=$(ssh ol-sys0 "sudo cat /etc/target/saveconfig.json | jq -r '.targets[0].wwn'")
$ sudo sed -i "/InitiatorName=/ s/=.*/=$WWN/" /etc/iscsi/initiatorname.iscsi
$ sudo systemctl enable --now iscsid.service
$ sudo iscsiadm -m discovery -t st -p $(ssh ol-sys0 hostname -i)
$ 
```

###

```
$ dnf install -y ovirt-hosted-engine-setup ovirt-engine-appliance
$ dnf install -y cockpit-ovirt-dashboard
$ firewall-cmd --permanent --add-port=9090/tcp
$ firewall-cmd --reload
$ systemctl enable --now cockpit.socket
$ hosted-engine --deploy --4
```