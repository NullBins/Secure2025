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
- [ A-RT2 ]
```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.127
ip dhcp excluded-address 192.168.10.160 192.168.10.254
ip dhcp excluded-address 192.168.20.1 192.168.20.99
ip dhcp excluded-address 192.168.20.151 192.168.20.254
ip dhcp pool A1-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.254
 dns-server 27.0.5.1
 domain-name cyber.net
ip dhcp pool A2-POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.254
 dns-server 27.0.5.1
 domain-name cyber.net
ip route 192.168.10.0 255.255.255.0 27.0.2.2 
```
- [ A-RT1 ]
```bash
interface GigabitEthernet0/0.99
 ip helper-address 27.0.2.1
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
 clock rate 2000000
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
 clock rate 2000000
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
 deny ip 192.168.20.0 0.0.0.255 27.0.5.0 0.0.0.3
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
vlan 99
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
ip ssh version 2
crypto key generate rsa general-keys modules 1024
username ssh-admin privilege 15 secret cyber2025!@
ip access-list standard SSH-ACCESS
 permit 192.168.10.128 0.0.0.31
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
 permit ospf any any
 permit tcp any host 27.0.7.1 eq www
 permit tcp any host 27.0.7.1 eq 443
 deny ip any any
ip inspect name outbound http timeout 3600
ip inspect name outbound icmp timeout 10
ip inspect name outbound udp timeout 30
ip inspect name outbound tcp timeout 3600
interface GigabitEthernet0/1
 ip access-group permit-rule in
 ip inspect outbound out
```

## 7. GRE over IPsec VPN 설정 (GRE-over-IPsec VPN configuration)
### < *Configuration* >
- [ A-RT2 ]
```bash
interface Tunnel0
 ip address 30.30.30.1 255.255.255.252
 mtu 1476
 tunnel source Serial0/0/0
 tunnel destination 27.0.4.1
ip route 27.0.5.0 255.255.255.252 30.30.30.2
crypto isakmp policy 5
 encr 3des
 hash md5
 authentication pre-share
 group 2
crypto isakmp key cyber123 address 27.0.4.1
ip access-list extended GRE-ACL
 permit gre host 27.0.2.1 host 27.0.4.1
crypto ipsec transform-set TS esp-3des esp-md5-hmac
crypto map cMAP 10 ipsec-isakmp 
 set peer 27.0.4.1
 set transform-set TS 
 match address GRE-ACL
interface Serial0/0/0
 crypto map cMAP
```
- [ ISP ]
```bash
interface Tunnel0
 ip address 30.30.30.2 255.255.255.252
 mtu 1476
 tunnel source Serial0/0/1
 tunnel destination 27.0.2.1
ip route 192.168.20.0 255.255.255.0 30.30.30.1
crypto isakmp policy 5
 encr 3des
 hash md5
 authentication pre-share
 group 2
crypto isakmp key cyber123 address 27.0.2.1
ip access-list extended GRE-ACL
 permit gre host 27.0.4.1 host 27.0.2.1
crypto ipsec transform-set TS esp-3des esp-md5-hmac
crypto map cMAP 10 ipsec-isakmp 
 set peer 27.0.2.1
 set transform-set TS 
 match address GRE-ACL
interface Serial0/0/1
 crypto map cMAP
```

## 8. B-FW 설정 (ASA 5505 Firewall settings)
### < *Configuration* >
- [ B-FW ]
```bash
interface GigabitEthernet1/1
 nameif OUTSIDE
 security-level 100
interface GigabitEthernet1/2
 nameif INSIDE
 security-level 0
interface GigabitEthernet1/2
 nameif INSIDE
 security-level 0
access-list OUTSIDE_IN extended permit icmp any any
access-list OUTSIDE_IN extended permit udp any any eq domain
access-list OUTSIDE_IN extended permit tcp any any eq www
access-list OUTSIDE_IN extended deny ip any any
access-group OUTSIDE_IN in interface OUTSIDE
dhcpd dns 10.10.10.1
dhcpd address 172.20.30.100-172.20.30.120 INSIDE
dhcpd domain cyber.net interface INSIDE
dhcpd enable INSIDE
interface GigabitEthernet1/1
 ospf cost 10
router ospf 10
 network 27.0.3.1 255.255.255.255 area 0
```

## 9. Radius 원격접속 설정 (Radius Remote access settings)
### < *Configuration* >
- [ B-RT ]
```bash
aaa new-model
aaa authentication login default group radius local
radius server RADIUS
 address ipv4 27.0.5.1 auth-port 1645
 key cyber123
radius server 27.0.5.1
 address ipv4 27.0.5.1 auth-port 1645
 key cyber123
ip ssh version 2
ip domain-name cyber.net
crypto key generate rsa general-keys modules 1024
line vty 0 4
 login authentication default
 transport input ssh
line con 0 4
 login authentication default
```
