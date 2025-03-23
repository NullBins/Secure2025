# 🛡 사이버 보안 *Cyber Security* 🔐
###### ⚔ Worldskills Korea 2025 Cyber Security Assignment Practice [ *Written by NullBins* ]
- By default, the commands are executed as a root user.

# [ *Project-3* ] <*Infrastructure configuration & Security enhancements*>

## 0. 기본 설정 (Default settings)
### < *Configuration* >
- [ server ]
```vim
ls -las /etc/update-motd.d/
chmod -x /etc/update-motd.d/10-help-text
chmod -x /etc/update-motd.d/50-motd-news
chmod -x /etc/update-motd.d/90-updates-available
chmod -x /etc/update-motd.d/91-release-upgrade
chmod -x /etc/update-motd.d/92-unattended-upgrades
systemctl disable systemd-networkd-wait-online.service
systemctl mask systemd-networkd-wait-online.service
```
```vim
vim /etc/vim/vimrc
```
>```vim
>set number
>set ignorecase
>```
```vim
vim /etc/sysctl.conf
```
>```vim
>net.ipv4.ip_forward=1
>net.ipv6.conf.all.disable_ipv6 = 1  
>net.ipv6.conf.default.disable_ipv6 = 1  
>net.ipv6.conf.lo.disable_ipv6 = 1
>```
```vim
vim /etc/issue
```
>```vim
>
>* Welcome to Cyber Security Environments ( Worldskills Korea 2025 | [Ubuntu Linux 24.04] ) *
>
>```
```vim
mv /etc/netplan/50-cloud-init.yaml /etc/netplan/config.yaml
vim /etc/netplan/config.yaml
```
>```yaml
>network:
>  ethernets:
>    ens32:
>      dhcp4: false
>  renderer: networkd
>  version: 2
>```
```vim
vim /etc/ssh/sshd_config
```
>```vim
>Port 22
>PermitRootLogin yes
>Banner /etc/issue
>```
```vim
systemctl restart ssh
systemctl enable ssh
```
```vim
timedatectl set-timezone Asia/Seoul
hostnamectl set-hostname security
sed -i "s/server/security/g" /etc/hosts
vim /etc/rc.local
```
>```vim
>#!/bin/bash
>
>sleep 10; sysctl -p > /var/log/rc-local;
>```
```vim
chmod +x /etc/rc.local
touch /var/log/rc-local
systemctl enable rc-local
systemctl restart rc-local
```

## 1. 호스트명 변경 및 IP 설정 (Change the hostname & IPv4 Address settings)
### < *Configuration* >
- [ fw ] - *Default configuration (Hostname & Network)*
```vim
hostnamectl set-hostname fw
sed -i "s/security/fw/g" /etc/hosts
modprobe 8021q
echo "8021q" | tee -a /etc/modules
ip link show | grep "link/ether" >> /etc/udev/rules.d/70.rules
vim /etc/udev/rules.d/70.rules
```
>```vim
>ACTION=="add",SUBSYSTEM=="net",ATTR{address}=="(macAddress|ens32)",NAME="eth1"
>ACTION=="add",SUBSYSTEM=="net",ATTR{address}=="(macAddress|ens33)",NAME="eth2"
>ACTION=="add",SUBSYSTEM=="net",ATTR{address}=="(macAddress|ens34)",NAME="eth3"
>```
```vim
vim /etc/default/grub
```
>```vim
>GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
>```
```vim
reboot
```
```vim
vim /etc/netplan/config.yaml
```
>```yaml
>network:
>  ethernets:
>    eth1:
>      dhcp4: false
>    eth2:
>      addresses: [10.30.30.6/29]
>      dhcp4: false
>    eth3:
>      dhcp4: true
>  vlans:
>    vlan.10:
>      id: 10
>      link: eth1
>      addresses: [192.168.70.1/25]
>    vlan.20:
>      id: 20
>      link: eth1
>      addresses: [192.168.70.254/25]
>  renderer: networkd
>  version: 2
>```
```vim
netplan apply
```
- [ isp ] - *Default configuration (Hostname & Network)*
```vim
hostnamectl set-hostname isp
sed -i "s/security/isp/g" /etc/hosts
modprobe 8021q
echo "8021q" | tee -a /etc/modules
ip link show | grep "link/ether" >> /etc/udev/rules.d/70.rules
vim /etc/udev/rules.d/70.rules
```
>```vim
>ACTION=="add",SUBSYSTEM=="net",ATTR{address}=="(macAddress|ens32)",NAME="eth1"
>ACTION=="add",SUBSYSTEM=="net",ATTR{address}=="(macAddress|ens33)",NAME="eth2"
>```
```vim
vim /etc/default/grub
```
>```vim
>GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
>```
```vim
reboot
```
```vim
vim /etc/netplan/config.yaml
```
>```yaml
>network:
>  ethernets:
>    eth1:
>      dhcp4: false
>    eth2:
>      addresses: [200.10.10.14/28]
>      dhcp4: false
>  vlans:
>    vlan.40:
>      id: 40
>      link: eth1
>      addresses: [180.20.10.1/28]
>    vlan.50:
>      id: 50
>      link: eth1
>      addresses: [180.20.10.65/28]
>  renderer: networkd
>  version: 2
>```
```vim
netplan apply
```
- [ exsrv ] - *Default configuration (Hostname & Network)*
```vim
hostnamectl set-hostname exsrv
sed -i "s/security/exsrv/g" /etc/hosts
modprobe 8021q
echo "8021q" | tee -a /etc/modules
```
```vim
vim /etc/netplan/config.yaml
```
>```yaml
>network:
>  ethernets:
>    ens32:
>      dhcp4: false
>  vlans:
>    vlan.50:
>      id: 50
>      link: ens32
>      addresses: [180.20.10.70/28]
>      routes:
>        - to: default
>          via: 180.20.10.65
>      nameservers:
>        addresses: [180.20.10.70]
>  renderer: networkd
>  version: 2
>```
```vim
netplan apply
```

## 2. DHCP 서버 구성 (DHCP Server configurations)
### < *Configuration* >
- [ exsrv ] - *ISC DHCP Server configurations*
```vim
vim /etc/default/isc-dhcp-server
```
>```vim
>INTERFACESv4="vlan.50"
>#INTERFACESv6=""
>```
```vim
vim /etc/dhcp/dhcpd.conf
```
>```vim
>subnet 200.10.10.0 netmask 255.255.255.240 {
>range 200.10.10.1 200.10.10.3;
>option routers 200.10.10.14;
>#option domain-name "";
>#option domain-name-servers;
>default-lease-time 600;
>max-lease-time 7200;
>}
>
>subnet 180.20.10.0 netmask 255.255.255.240 {
>range 180.20.10.4 180.20.10.7;
>option routers 180.20.10.1;
>option domain-name "korea.com";
>option domain-name-servers 180.20.10.70;
>default-lease-time 600;
>max-lease-time 7200;
>}
>
>subnet 180.20.10.64 netmask 255.255.255.240 {
>}
>
>host fw {
>hardware ethernet (00:0c:29):(fw_mac);
>fixed-address 200.10.10.1;
>}
>```
```vim
systemctl enable isc-dhcp-server
systemctl restart isc-dhcp-server
```
- [ isp ] - *ISC DHCP Relay configurations*
```vim
vim /etc/default/isc-dhcp-relay
```
>```vim
>SERVERS="180.20.10.70"
>```
```vim
systemctl enable isc-dhcp-relay
systemctl restart isc-dhcp-relay
```
