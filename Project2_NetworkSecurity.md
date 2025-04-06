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

## 2. Frame-Relay 라우터 설정 (FR Router settings | Hub-and-Spoke | No Sub-iface)
### < *Configuration* >
- [ A-RT1 ]
```bash
interface Serial0/0/0
 ip address 27.0.1.1 255.255.255.0
 encapsulation frame-relay
 frame-relay map ip 27.0.1.3 103 broadcast
 frame-relay map ip 27.0.1.2 103 broadcast
 no frame-relay inverse-arp
 ip ospf network broadcast
 ip ospf priority 0
```
- [ B-RT ]
```bash
interface Serial0/3/0
 ip address 27.0.1.2 255.255.255.0
 encapsulation frame-relay
 frame-relay map ip 27.0.1.3 203 broadcast
 frame-relay map ip 27.0.1.1 203 broadcast
 no frame-relay inverse-arp
 ip ospf network broadcast
 ip ospf priority 0
```
- [ C-RT ]
```bash
interface Serial0/1/0
 ip address 27.0.1.3 255.255.255.0
 encapsulation frame-relay
 frame-relay map ip 27.0.1.1 301 broadcast
 frame-relay map ip 27.0.1.2 302 broadcast
 no frame-relay inverse-arp
 ip ospf network broadcast
 ip ospf priority 10
```

## 3. OSPF 라우팅 설정 (OSPFv4 Routing)
### < *Configuration* >
- [ A-RT1 ]
```bash
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
router ospf 10
 router-id 1.1.1.1
 log-adjacency-changes
 network 27.0.1.1 0.0.0.0 area 0
 network 27.0.2.2 0.0.0.0 area 0
```
- [ A-RT2 ]
```bash
router ospf 10
 log-adjacency-changes
 network 27.0.2.1 0.0.0.0 area 0
```
- [ B-RT ]
```bash
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
router ospf 10
 router-id 2.2.2.2
 log-adjacency-changes
 network 27.0.1.2 0.0.0.0 area 0
```
- [ C-RT ]
```bash
interface Loopback0
 ip address 3.3.3.3 255.255.255.255
router ospf 10
 router-id 3.3.3.3
 log-adjacency-changes
 network 27.0.1.3 0.0.0.0 area 0
 network 27.0.6.129 0.0.0.0 area 0
 network 27.0.4.2 0.0.0.0 area 0
```
- [ ISP ]
```bash
router ospf 10
 log-adjacency-changes
 network 27.0.4.1 0.0.0.0 area 0
 network 27.0.5.2 0.0.0.0 area 0
 network 27.0.6.2 0.0.0.0 area 0
```
- [ SH-SW ]
```bash
ip routing
router ospf 10
 log-adjacency-changes
 network 27.0.7.126 0.0.0.0 area 0
 network 27.0.7.254 0.0.0.0 area 0
 network 27.0.6.254 0.0.0.0 area 0
```

## 3. NAT 설정 (Network Address Translation)
### < *Configuration* >
- [ A-RT1 ]
```bash
interface Serial0/0/0
 ip nat outside
interface GigabitEthernet0/0
 ip nat inside
ip access-list extended IN-NET
 permit ip 192.168.10.0 0.0.0.255 any
ip nat inside source list IN-NET interface Serial0/0/0 overload
ip classless
```
- [ A-RT2 ]
```bash
interface GigabitEthernet0/0
 ip nat inside
interface Serial0/0/0
 ip nat outside
ip access-list extended IN-NET
 permit ip 192.168.20.0 0.0.0.255 any
ip nat inside source list IN-NET interface Serial0/0/0 overload
ip classless
```

## 4. Home-Router settings (Wireless LAN Router settings)
### < *Configuration* >
- [ HOME-RT ]

> ![Image](https://github.com/NullBins/Secure2025/blob/main/IMG/home-rt-dhcp1.png)

> ![Image](https://github.com/NullBins/Secure2025/blob/main/IMG/home-rt-dhcp2.png)
