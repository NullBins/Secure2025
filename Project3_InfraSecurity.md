# 🛡 사이버 보안 *Cyber Security* 🔐
###### ⚔ Worldskills Korea 2025 Cyber Security Assignment Practices 🗡 [ *Written by NullBins* ]
- By default, the commands are executed as a root user.

# [ *Project-3* ] <*🌐Infrastructure configuration & Security enhancements💠*>

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
vim /etc/hosts
```
>```vim
>127.0.1.1 exsrv.korea.com exsrv
>```
```vim
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
- [ ns ] - *Default configuration (Hostname & Network)*
```vim
hostnamectl set-hostname ns
sed -i "s/security/ns/g" /etc/hosts
vim /etc/hosts
```
>```vim
>127.0.1.1 ns.cyber.net ns
>```
```vim
vim /etc/netplan/config.yaml
```
>```yaml
>network:
>  ethernets:
>    ens32:
>      addresses: [10.30.30.1/29]
>      routes:
>        - to: default
>          via: 10.30.30.6
>      nameservers:
>        addresses: [200.10.10.1]
>      dhcp4: false
>  renderer: networkd
>  version: 2
>```
```vim
netplan apply
```
- [ www ] - *Default configuration (Hostname & Network)*
```vim
hostnamectl set-hostname www
sed -i "s/security/www/g" /etc/hosts
vim /etc/hosts
```
>```vim
>127.0.1.1 www.cyber.net www
>```
```vim
vim /etc/netplan/config.yaml
```
>```yaml
>network:
>  ethernets:
>    ens32:
>      addresses: [10.30.30.2/29]
>      routes:
>        - to: default
>          via: 10.30.30.6
>      nameservers:
>        addresses: [10.30.30.1]
>      dhcp4: false
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
>option routers 180.20.10.65;
>}
>
>host fw {
>hardware ethernet [fw mac address];
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
- [ ns ] - *ISC DHCP Server configurations*
```vim
vim /etc/default/isc-dhcp-server
```
>```vim
>INTERFACESv4="ens32"
>#INTERFACESv6=""
>```
```vim
vim /etc/dhcp/dhcpd.conf
```
>```vim
>subnet 192.168.70.0 netmask 255.255.255.128 {
>range 192.168.70.64 192.168.70.126;
>option routers 192.168.70.1;
>option domain-name "cyber.net";
>option domain-name-servers 10.30.30.1;
>default-lease-time 600;
>max-lease-time 7200;
>}
>
>subnet 192.168.70.128 netmask 255.255.255.128 {
>range 192.168.70.129 192.168.70.191;
>option routers 192.168.70.254;
>option domain-name "cyber.net";
>option domain-name-servers 10.30.30.1;
>default-lease-time 600;
>max-lease-time 7200;
>}
>
>subnet 10.30.30.0 netmask 255.255.255.248 {
>option routers 10.30.30.6;
>}
>```
```vim
systemctl enable isc-dhcp-server
systemctl restart isc-dhcp-server
```
- [ fw ] - *ISC DHCP Relay configurations*
```vim
vim /etc/default/isc-dhcp-relay
```
>```vim
>SERVERS="10.30.30.1"
>```
```vim
systemctl enable isc-dhcp-relay
systemctl restart isc-dhcp-relay
```

## 3. FW NAT 구성 (FW NAT Configurations | Masquerade & Port Forwarding)
### < *Configuration* >
- [ fw ] - *UFW(Uncomplicated Firewall) NAT Configurations*
```vim
vim /etc/default/ufw
```
>```vim
>DEFAULT_INPUT_POLICY="DROP"
>DEFAULT_FORWARD_POLICY="ACCEPT"
>```
```vim
vim /etc/ufw/before.rules
```
>```vim
>*nat
>:PREROUTING ACCEPT [0:0]
>:POSTROUTING ACCEPT [0:0]
>
>-A PREROUTING -d 200.10.10.1 -p tcp --dport 80 -j DNAT --to 10.30.30.2
>-A PREROUTING -d 200.10.10.1 -p tcp --dport 443 -j DNAT --to 10.30.30.2
>-A POSTROUTING -s 192.168.70.0/24 -o eth3 -j MASQUERADE
>-A POSTROUTING -s 10.30.30.0/29 -o eth3 -j MASQUERADE
>
>COMMIT
>```
```vim
ufw disable
ufw enable
ufw status
systemctl restart ufw
iptables -t nat -L -n
```

## 4. FW 방화벽 구성 (FW Firewall Configurations)
### < *Configuration* >
- [ fw ] - *UFW(Uncomplicated Firewall) Firewall Configurations*
```vim
ufw default deny incoming
ufw allow in on eth3 proto tcp to any port 80
ufw allow in on eth3 proto tcp to any port 443
ufw allow in on vlan.10 proto tcp to any port 20250
ufw allow in proto udp to any port 67
ufw allow in proto udp to any port 68
ufw reload
ufw status verbose
```

## 5. 도메인 서비스 구성 (Domain Service Configurations)
### < *Configuration* >
- [ ns ] - *BIND9(Named Service) Configurations*
```vim
vim /etc/bind/named.conf
```
>```vim
>#include "/etc/bind/named.conf.options";
>#include "/etc/bind/named.conf.local";
>#include "/etc/bind/named.conf.default-zones";
>
>options {
>  directory "/var/cache/bind";
>  listen-on { any; };
>  allow-query { any; };
>  recursion yes;
>  dnssec-validation no;
>};
>
>zone "cyber.net" {
>  type master;
>  file "cyber.zone";
>};
>```
```vim
cp /etc/bind/db.0 /var/cache/bind/cyber.zone
chown bind:bind -R /var/cache/bind/
sed -i "s/localhost/ns.cyber.net/g" /var/cache/bind/cyber.zone
vim /var/cache/bind/cyber.zone
```
>```vim
>@    IN    NS    ns.cyber.net.
>ns    IN    A    10.30.30.1
>www    IN    A    10.30.30.2
>```
```vim
systemctl enable named
systemctl restart named
```
- [ exsrv ] - *BIND9(Named Service) Configurations*
```vim
vim /etc/bind/named.conf
```
>```vim
>#include "/etc/bind/named.conf.options";
>#include "/etc/bind/named.conf.local";
>#include "/etc/bind/named.conf.default-zones";
>
>options {
>  directory "/var/cache/bind";
>  listen-on { any; };
>  allow-query { any; };
>  recursion yes;
>  dnssec-validation no;
>};
>
>zone "korea.com" {
>  type master;
>  file "korea.zone";
>};
>
>zone "cyber.net" {
>  type master;
>  file "cyber.zone";
>};
>```
```vim
cp /etc/bind/db.0 /var/cache/bind/korea.zone
cp /etc/bind/db.0 /var/cache/bind/cyber.zone
chown bind:bind -R /var/cache/bind/
sed -i "s/localhost/exsrv.cyber.net/g" /var/cache/bind/korea.zone
sed -i "s/localhost/www.cyber.net/g" /var/cache/bind/cyber.zone
vim /var/cache/bind/korea.zone
```
>```vim
>@    IN    NS    www.korea.com.
>www    IN    A    200.10.10.1
>```
```vim
vim /var/cache/bind/cyber.zone
```
>```vim
>@    IN    NS    exsrv.cyber.net.
>exsrv    IN    A    180.20.10.70
>www    IN    A    180.20.10.70
>```
```vim
systemctl enable named
systemctl restart named
```

## 6. 웹 서비스 및 보안 구성 (Web Service & Security Configurations)
### < *Configuration* >
- [ www ] - *Apache2 Service (Wordpress Engine) Configurations*
```vim
cd /home/cyber/wordpress
gunzip ./latest.tar.gz
tar -xvf ./latest.tar
mv -iv ./wordpress /var/www/html/
cp /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
cat ./salt >> /var/www/html/wordpress/wp-config.php
mysql -u root
  mysql> create database wordpress;
  mysql> create user 'wp_user'@'localhost' identified by 'cyber2025!@';
  mysql> grant all privileges on wordpress.* to 'wp_user'@'localhost';
  mysql> flush privileges;
  mysql> exit
vim /var/www/html/wordpress/wp-config.php
```
>```php
>define('DB_NAME', 'wordpress');
>define('DB_USER', 'wp_user');
>define('DB_PASSWORD', 'cyber2025!@');
>define('DB_HOST', 'localhost');
>```
```vim
vim /etc/apache2/apache2.conf
```
>```vim
><Directory /var/www/>
>  Options -Indexes
>  AllowOverride None
>  Require all granted
></Directory>
>```
```vim
a2enmod ssl
a2ensite default-ssl.conf
vim /etc/apache2/sites-available/000-default.conf
```
>```vim
><VirtualHost: *:80>
>  ServerName www.cyber.net
>  ServerAdmin cyber_admin@cyber.net
>  DocumentRoot /var/www/html/wordpress
>  Redirect permanent / https://www.cyber.net/
></VirtualHost>
>```
```vim
vim /etc/apache2/sites-available/default-ssl.conf
```
>```vim
><VirtualHost: *:443>
>  ServerName www.cyber.net
>  ServerAdmin cyber_admin@cyber.net
>  DocumentRoot /var/www/html/wordpress
>  SSLCertificateFile /etc/ssl/certs/ssl-cert-snakeoil.pem
>  SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
></VirtualHost>
>```
```vim
systemctl enable apache2
systemctl restart apache2
```

## 7. FW SSH 설정 및 보안 강화 (FW SSH Configuration & Security Enhancements)
### < *Configuration* >
- [ user01 ] - *SSH Keypair Keygens*
```vim
ssh-keygen -t rsa -b 4096 -C "fw.key"
ssh-copy-id cyber@192.168.70.1
```
- [ fw ] - *SSH Service Security Configurations*
```vim
vim /etc/ssh/sshd_config
```
>```vim
>Port 20250
>AllowUsers cyber@192.168.70.0/25
>PasswordAuthentication no
>PubkeyAuthentication yes
>#PermitRootLogin yes
>#UsePAM yes
>#PrintMotd no
>```
```vim
systemctl restart ssh
```
- [ user01 ] - *SSH Service Connection Script*
```vim
vim /home/cyber/fw.sh
```
>```vim
>#!/bin/bash
>ssh -p 20250 cyber@192.168.70.1
>```
```vim
chown cyber:cyber /home/cyber/fw.sh
chmod 700 /home/cyber/fw.sh
./fw.sh
```

## 8. WWW SSH 보안 강화 스크립트 (WWW SSH Security Script)
### < *Configuration* >
- [ www ] - *Fail2Ban Service Configuration & SSH Shell Scripting*
```vim
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
vim /etc/fail2ban/jail.local
```
>```vim
>[sshd]
>enabled = true
>port = ssh
>filter = sshd
>logpath = /var/log/auth.log
>maxretry = 5
>findtime = 300
>bantime = 600
>ignoreip = 192.168.70.0/25
>```
```vim
systemctl enable fail2ban
systemctl restart fail2ban
vim /home/cyber/ssh-log.sh
```
>```vim
>#!/bin/bash
>
>LOG_DIR="/home/cyber/ssh-log"
>CDATE=$(date + "%Y-%m-%d %H:%M")
>DATE_FORMAT=$(date + "%Y%m%d")
>TIME_FORMAT=$(date + "%H%M")
>LOG_FILE="${LOG_DIR}/${DATE_FORMAT}_${TIME_FORMAT}.log"
>
>if [ ! -d "$LOG_DIR" ]; then
>  mkdir -p "$LOG_DIR"
>fi
>
>echo "" | tee "$LOG_FILE"
>echo "DATE: $CDATE" | tee -a "$LOG_FILE"
>echo "" | tee -a "$LOG_FILE"
>
># Ban IP List
>echo "[Ban IP]" | tee -a "$LOG_FILE"
>grep "Ban " /var/log/fail2ban.log | grep "sshd" | awk '{print $NF}' | sort | uniq -c | while read count ip; do
>echo -e "\t$count $ip [sshd]" | tee -a "$LOG_FILE"
>done
>echo "" | tee -a "$LOG_FILE"
>
># Unban IP List
>echo "[Unban IP]" | tee -a "$LOG_FILE"
>grep "Unban " /var/log/fail2ban.log | grep "sshd" | awk '{print $NF}' | sort | uniq -c | while read count ip; do
>echo -e "\t$count $ip [sshd]" | tee -a "$LOG_FILE"
>done
>echo "" | tee -a "$LOG_FILE"
>
># Access Network
>echo "[My Net Access IP]" | tee -a "LOG_FILE"
>grep "Ignore " /var/log/fail2ban.log | grep "sshd" | awk '{print $8}' | sort | uniq -c | while read count ip; do
>echo -e "\t$count $ip [sshd]" | tee -a "$LOG_FILE"
>done
>echo "" | tee -a "$LOG_FILE"
>
># ETC Connect IP List
>echo "[Access IP]" | tee -a "$LOG_FILE"
>grep "Accepted " /var/log/auth.log | grep "sshd" | awk '{print $9}' | sort | uniq -c | while read count ip; do
>echo -e "\t$count $ip [sshd]" | tee -a "$LOG_FILE" 
>done
>echo "" | tee -a "$LOG_FILE"
>
># Permissions
>chmod 700 "$LOG_FILE"
>```
```vim
chown cyber:cyber /home/cyber/ssh-log.sh
chmod 750 /home/cyber/ssh-log.sh
```
