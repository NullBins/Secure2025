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
```
- [ server ]
>```vim
>
>```

## 1. IPv6 프로토콜 제거 (Disable IPv6 protocol)
### < *Configuration* >
```vim
vim /etc/sysctl.conf
```
- [ All hosts ]
>```vim
>net.ipv6.conf.all.disable_ipv6 = 1  
>net.ipv6.conf.default.disable_ipv6 = 1  
>net.ipv6.conf.lo.disable_ipv6 = 1
>```
- [ fw01 ]
>```vim
>net.ipv4.ip_forward = 1  
>```
```vim
sysctl -p
```
### < *Checking* >
```vim
ip -6 addr show | grep "inet6"
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
