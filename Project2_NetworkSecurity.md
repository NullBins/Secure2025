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

## 4. Home-Router 설정 (Wireless LAN Router settings)
### < *Configuration* >
- [ HOME-RT ]

> ![Image](https://github.com/NullBins/Secure2025/blob/main/IMG/home-rt-dhcp1.png)

> ![Image](https://github.com/NullBins/Secure2025/blob/main/IMG/home-rt-dhcp2.png)

## 5. A-SW 보안 설정 (A-SW Security settings)
### < *Configuration* >
- [ A-SW ]
```bash
configure terminal
 vlan 99
exit
interface vlan 99
 ip address 192.168.10.253 255.255.255.0
interface fa0/1
 switchport mode access
 switchport access vlan 99
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
ip domain-name cyber.net
crypto key generate rsa general-keys modules 1024
username ssh-admin privilege 15 secret cyber2025!@
ip access-list standard SSH-ACCESS
 permit 192.168.10.128 0.0.0.31
ip ssh version 2
line vty 0 4
 transport input ssh
 access-class SSH-ACCESS in
 login local
```

## 6. SH-SW 보안 설정 (SH-SW Security settings)
### < *Configuration* >
- [ A-SW ]
```bash
ip access-list extended permit-rule
 deny ip any any
 permit ospf any any
 permit tcp any host 27.0.7.1 eq www
 permit icmp any any echo-reply
 permit tcp any any eq www established
 permit udp any any eq domain
interface GigabitEthernet0/1
 ip access-group permit-rule in
```

## 7. GRE over IPsec VPN 설정 (GRE-over-IPsec VPN configuration)
### < *Configuration* >
- [ A-RT2 ]
```bash
interface Tunnel0
 ip address 30.30.30.1 255.255.255.252
 tunnel mode gre ip
 tunnel source Serial0/0/0
 tunnel destination 27.0.4.1
ip route 27.0.5.0 0.0.0.3 tunnel 0
license boot module c2900 technology-package securityk9
crypto isakmp policy 5
 encr 3des
 hash md5
 authentication pre-share
 group 2
crypto isakmp key cyber123 address 27.0.4.1
ip access-list extended GRE-ACL
 permit gre host 30.30.30.1 host 30.30.30.2
crypto ipsec transform-set TS esp-3des esp-md5-hmac
crypto map cMAP 10 ipsec-isakmp 
 set peer 27.0.4.1
 set transform-set TS 
 match address GRE-ACL
interface Serial0/0/0
 no ip nat outside
 crypto map cMAP
ip access-list extended IN-NET
 deny ip 192.168.20.0 0.0.0.255 27.0.5.0 0.0.0.3
```
- [ ISP ]
```bash
interface Tunnel0
 ip address 30.30.30.1 255.255.255.252
 tunnel mode gre ip
 tunnel source Serial0/0/1
 tunnel destination 27.0.2.1
ip route 192.168.20.0 0.0.0.255 tunnel 0
license boot module c2900 technology-package securityk9
crypto isakmp policy 5
 encr 3des
 hash md5
 authentication pre-share
 group 2
crypto isakmp key cyber123 address 27.0.2.1
ip access-list extended GRE-ACL
 permit gre host 30.30.30.2 host 30.30.30.1
crypto ipsec transform-set TS esp-3des esp-md5-hmac
crypto map cMAP 10 ipsec-isakmp 
 set peer 27.0.2.1
 set transform-set TS 
 match address GRE-ACL
interface Serial0/0/1
 crypto map cMAP
```
