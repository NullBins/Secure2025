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

## 2. 호스트명 변경 및 IP 설정 (Change the hostname & IPv4 Address settings)
### < *Configuration* >
* Hostname (All hosts)
```vim
hostnamectl set-hostname <Hostname to change>
cat /etc/hostname
sed -i "s/<Original hostname>/<Hostname to change>/g" /etc/hosts
```
* IPv4 Address (All hosts)
```bash
mv /etc/netplan/*.yaml /etc/netplan/config.yaml   ### 채점기준에 요구되는 파일명으로 변경
ip link show   ### 인터페이스 확인 (ens33 ~ 4 . . . | VMware)
```
```vim
vim /etc/netplan/config.yaml
```
>```yaml
>networks:
>  ethernets:
>    ens33:
>      addresses: [10.10.10.10/24]
>      routes:
>        - to: default
>          via: 10.10.10.1
>      nameservers:
>        addresses: [10.10.10.12]
>      dhcp4: false
>  version: 2
>```
```bash
netplan apply
```
### < *Checking* >
```vim
hostname
ip -4 addr show | grep "global"
route -n4 | grep "G"
```
