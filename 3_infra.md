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
systemctl unmask systemd-networkd-wait-online.service
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
>Welcome to Cyber Security 2025 (Ubuntu Linux 24.04)
>
>```
```vim
mv /etc/netplan/50-cloud-init.yaml /etc/netplan/config.yaml
vim /etc/netplan/config.yaml
```
>```yaml
>networks:
>  ethernets:
>    ens33:
>      dhcp4: false
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
hostnamectl set-hostname secure
vim /etc/hosts
```
>```vim
>127.0.1.1 secure secure
>```
```vim
timedatectl set-timezone Asia/Seoul
```

## 1. VLAN 설정 (VLAN settings)
### < *Configuration* >
- [ fw ]
```vim
modprobe 8021q
echo "8021q" | tee -a /etc/modules
```
* Network Configuration
```vim
vim /etc/netplan/config.yaml
```
>```vim
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
>    vlan10:
>      id: 10
>      link: eth1
>      addresses: [192.168.70.1/25]
>    vlan20:
>      id: 20
>      link: eth1
>      addresses: [192.168.70.254/25]
>  version: 2
>  renderer: networkd
>```
```vim
cat /sys/class/net/vlan*/ifindex >> /etc/udev/rules.d/70.rules
vim /etc/udev/rules.d/70.rules
```
>```vim
>ACTION=="add",SUBSYSTEM=="net",ATTR{ifindex}=="(ifindex)",NAME=""
>```
