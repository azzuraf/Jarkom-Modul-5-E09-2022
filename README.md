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
