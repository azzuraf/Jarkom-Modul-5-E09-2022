# Jarkom-Modul-5-E09-2022

## Anggota Kelompok
| Nama | NRP |
| ------------- | ------------- |
|  Lia Kharisma Putri | 5025201034 |
|  Azzura Ferliani Ramadhani  | 5025201190 |
|  Ingwer Ludwig Nommensen | 5025201259 |
## Subnetting
## IP Tree
## Configuration
#### Strix
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
hwaddress ether b6:23:6b:b5:5d:33

auto eth1
iface eth1 inet static
address 10.26.7.149
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 10.26.7.145
netmask 255.255.255.252
```
#### Westalis
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.26.7.146
netmask 255.255.255.252
gateway 10.26.7.145

auto eth1
iface eth1 inet static
address 10.26.0.1
netmask 255.255.252.0

auto eth2
iface eth2 inet static
address 10.26.7.1
netmask 255.255.255.128

auto eth3
iface eth3 inet static
address 10.26.7.129
netmask 255.255.255.248
```
#### Ostania
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.26.7.150
netmask 255.255.255.252
gateway 10.26.7.149

auto eth1
iface eth1 inet static
address 10.26.4.1
netmask 255.255.254.0

auto eth2
iface eth2 inet static
address 10.26.7.137
netmask 255.255.255.248

auto eth3
iface eth3 inet static
address 10.26.6.1
netmask 255.255.255.0
```
#### Eden
```
auto eth0
iface eth0 inet static
address 10.26.7.130
netmask 255.255.255.248
gateway 10.26.7.129
```
#### WISE
```
auto eth0
iface eth0 inet static
address 10.26.7.131
netmask 255.255.255.248
gateway 10.26.7.129
```
#### SSS
```
auto eth0
iface eth0 inet static
address 10.26.7.139
netmask 255.255.255.248
gateway 10.26.7.137
```
#### Garden
```
auto eth0
iface eth0 inet static
address 10.26.7.138
netmask 255.255.255.248
gateway 10.26.7.137
```
#### Blackbell, Briar, Desmond, Forger
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```
## Routing
#### Strix
```
route add -net 10.26.7.128 netmask 255.255.255.248 gw 10.26.7.146
route add -net 10.26.7.0 netmask 255.255.255.128 gw 10.26.7.146
route add -net 10.26.0.0 netmask 255.255.252.0 gw 10.26.7.146

route add -net 10.26.4.0 netmask 255.255.254.0 gw 10.26.7.150
route add -net 10.26.6.0 netmask 255.255.255.0 gw 10.26.7.150
route add -net 10.26.7.136 netmask 255.255.255.248 gw 10.26.7.150
```
## Eden Sebagai DNS Server
Eden akan berperan sebagai DNS Server, maka pada file ```/etc/bind/named.conf.options ``` akan ditambahkan command berikut:
```
forwarders {
     192.168.122.1;
};

allow-query{any;};

auth-nxdomain no;    # conform to RFC1035
listen-on-v6 { any; };
```
## WISE Sebagai DHCP Server
Untuk menjadikan WISe sebagai DHCP server, tahapannya adalah sebagai berikut:
- Pada file ``` bashrc ``` masukkan command berikut:
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```
- Tambahkan command ``` INTERFACES="eth0" ``` pada file ``` /etc/default/isc-dhcp-server ```
- Lalu pada file ``` /etc/dhcp/dhcpd.conf ``` masukkan command seperti berikut:
```
#Forger
subnet 10.26.7.0 netmask 255.255.255.128 {
    range 10.26.7.2 10.26.7.126;
    option routers 10.26.7.1;
    option broadcast-address 10.26.7.127;
    option domain-name-servers 10.26.7.130; 
    default-lease-time 600;
    max-lease-time 7200;
}

#Desmond
subnet 10.26.0.0 netmask 255.255.252.0 {
    range 10.26.0.2 10.26.3.254;
    option routers 10.26.0.1;
    option broadcast-address 10.26.3.255;
    option domain-name-servers 10.26.7.130;
    default-lease-time 600;
    max-lease-time 7200;
}

#Blackbell
subnet 10.26.4.0 netmask 255.255.254.0 {
    range 10.26.4.2 10.26.5.254;
    option routers 10.26.4.1;
    option broadcast-address 10.26.5.255;
    option domain-name-servers 10.26.7.130;
    default-lease-time 600;
    max-lease-time 7200;
}

#Briar
subnet 10.26.6.0 netmask 255.255.255.0 {
    range 10.26.6.2 10.26.6.254;
    option routers 10.26.6.1;
    option broadcast-address 10.26.6.255;
    option domain-name-servers 10.26.7.130;
    default-lease-time 600;
    max-lease-time 7200;
}

subnet 10.26.7.128 netmask 255.255.255.248 {
    option routers 10.26.7.129;
}
```
## Ostania dan Westalis Sebagai DHCP Relay
- Tambahkan command ``` apt-get install isc-dhcp-relay -y ``` pada file ``` bashrc ```
- Masukkan command berikut pada file ```/etc/default/isc-dhcp-relay```
```
# What servers should the DHCP relay forward requests to?
SERVERS="10.26.7.131"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
