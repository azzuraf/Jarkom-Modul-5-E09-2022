# Jarkom-Modul-5-E09-2022

## Anggota Kelompok
| Nama | NRP |
| ------------- | ------------- |
|  Lia Kharisma Putri | 5025201034 |
|  Azzura Ferliani Ramadhani  | 5025201190 |
|  Ingwer Ludwig Nommensen | 5025201259 |
## Subnetting
![](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/Subnetting.png)
## IP Tree
![](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/Tree.png)

![](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/Pembagian%20IP.png)
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
## Penyelesaian Soal
### Soal 1
**Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan MASQUERADE.** <br/><br/>
- Yang pertama masukan command ```ip a``` pada Strix untuk mendapatkan ip dari strix
![](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/IPstrix.png)
- selanjutnya lakukan testing di strix
<br/><br/>
![](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/no1_test.png)
### Soal 2
**Kalian diminta untuk melakukan drop semua TCP dan UDP dari luar Topologi kalian pada server yang merupakan DHCP Server demi menjaga keamanan.**<br/><br/>
Untuk menyelesaikannya, kita perlu memasukkan setup iptables pada WISE yang merupakan DHCP server.
```
iptables -A INPUT ! -s 10.26.0.0/21 -p tcp -j DROP
iptables -A INPUT ! -s 10.26.0.0/21 -p udp -j DROP
```
<br/>**Testing:**<br/><br/>
![Test no.2](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/no2.png)
### Soal 3
**Loid meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 2 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.**<br/><br/>
Pada Eden dan WISE akan dimasukkan setup iptables seperti berikut:
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
```
Dimana ```--connlimit-above 2``` digunakan untuk memberikan limit koneksi ICMP dan juga tidak lupa menambahkan ```DROP``` agar koneksi selain 2 koneksi tersebut didrop.<br/><br/>
**Testing:**<br/><br/>
![Test no.3 Desmond](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/no3_desmond.png)<br/>
![Test no.3 Briar](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/no3_briar.png)<br/>
![Test no.3 Blackbell](https://github.com/azzuraf/Jarkom-Modul-5-E09-2022/blob/main/file%20m5/no3_black.png)
### Soal 4
**Akses menuju Web Server hanya diperbolehkan disaat jam kerja yaitu Senin sampai Jumat pada pukul 07.00 - 16.00.** <br/><br/>
Hanya bisa diakses pada Senin-Kamis 07:00-16:00

**Perlu diketahuai bahwa** <br>
**Pada subnet 10.26.19.0/29 terdapat IP Garden, SSS, serta interface pada Ostania yang terhubung** <br>

**Pada Ostania :** <br>
```
iptables -A FORWARD -d 10.26.19.0/29 -m time --weekdays Sat,Sun -j REJECT
iptables -A FORWARD -d 10.26.19.0/29 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
iptables -A FORWARD -d 10.26.19.0/29 -m time --timestart 16:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
```
**Hati-hati karena interface Ostania yang terhubung ke Garden & SSS juga terikut tidak bisa dihubungi**

**Untuk mengubah date pada Ostania untuk testing gunakan perintah berikut**

```
date --set "16 Sep 2002 00:00:00"
```

### Soal 5
**Karena kita memiliki 2 Web Server, Loid ingin Ostania diatur sehingga setiap request dari client yang mengakses Garden dengan port 80 akan didistribusikan secara bergantian pada SSS dan Garden secara berurutan dan request dari client yang mengakses SSS dengan port 443 akan didistribusikan secara bergantian pada Garden dan SSS secara berurutan.** <br/><br/>

Karena ingin dibuat merata, maka kita tinggal mengubah destination address setiap paket kelipatan 2. Untuk mengubah destination address, digunakanlah PREROUTING

10.26.19.2 : IP Garden <br>
10.26.19.3 : IP SSS <br>

Pada Ostania : <br>
```
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.26.19.2 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.26.19.3:80

iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.26.19.3 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.26.19.2:443
```
