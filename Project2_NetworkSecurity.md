# 🛡 사이버 보안 *Cyber Security* 🔐
###### ⚔ Worldskills Korea 2025 Cyber Security Assignment Practices 🗡 [ *Written by NullBins* ]
- By default, the commands are executed as a root user.

# [ *Project-2* ] <*🌐Network security device settings💫*>

## 0. 기본 설정 (Default settings)
### < *Configuration* >
- [ All hosts ]
```bash
enable
configure terminal
hostname <hostname>
enable password cyber123
service password-encryption
```

## 1. DHCP 서버 설정 (DHCP Server settings)
### < *Configuration* >
- [ A-RT1 ]
```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.127
ip dhcp excluded-address 192.168.10.160 192.168.10.254
!
ip dhcp pool A1-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.254
 dns-server 27.0.5.1
 domain-name cyber.net
```
- [ A-RT2 ]
```bash
ip dhcp excluded-address 192.168.20.1 192.168.20.99
ip dhcp excluded-address 192.168.20.151 192.168.20.254
!
ip dhcp pool A2-POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.254
 dns-server 27.0.5.1
 domain-name cyber.net
```
- [ SH-SRV ]
> ![Image](https://github.com/NullBins/Secure2025/blob/main/IMG/sh-srv-dhcp.png)
