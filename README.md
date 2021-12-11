# Jarkom-Modul-5-E06-2021

## (A)
### Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy dibawah ini:
![image1](https://user-images.githubusercontent.com/36522826/145227758-2587b7e7-481f-4fde-9b20-26382f886a08.png)

Keterangan :
- Doriki adalah DNS Server
- Jipangu adalah DHCP Server
- Maingate dan Jorge adalah Web Server
- Jumlah Host pada Blueno adalah 100 host
- Jumlah Host pada Cipher adalah 700 host
- Jumlah Host pada Elena adalah 300 host
- Jumlah Host pada Fukurou adalah 200 host

## (B)
### Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting, 

### Solusi
**Disini kami menggunakan teknik VLSM untuk pembagian subnet, sehingga pembagian subnet menjadi:**

![image](https://user-images.githubusercontent.com/57354564/145662972-eba68124-6b59-4b0d-9e64-3cd70ba698ac.png)

| Network | Jumlah IP | Netmask |
|:-------:|:---------:|:-------:|
| A1      |         3 | /29     |
| A2      |       101 | /25     |
| A3      |       701 | /22     |
| A4      |         2 | /30     |
| A5      |         2 | /30     |
| A6      |       301 | /23     |
| A7      |       201 | /24     |
| A8      |         3 | /29     |
| Total   |      1314 | /21     |

| Name | Network Address | Slash |       Mask      |  Broadcast  |
|:----:|:---------------:|:-----:|:---------------:|:-----------:|
|  A1  |   10.32.7.128   |  /29  | 255.255.255.248 | 10.32.7.135 |
|  A2  |    10.32.7.0    |  /25  | 255.255.255.128 | 10.32.7.127 |
|  A3  |    10.32.0.0    |  /22  |  255.255.252.0  | 10.32.3.255 |
|  A4  |   10.32.7.144   |  /30  | 255.255.255.252 | 10.32.7.147 |
|  A5  |   10.32.7.148   |  /30  | 255.255.255.252 | 10.32.7.151 |
|  A6  |    10.32.4.0    |  /23  |  255.255.254.0  | 10.32.5.255 |
|  A7  |    10.32.6.0    |  /24  |  255.255.255.0  | 10.32.6.255 |
|  A8  |   10.32.7.136   |  /29  | 255.255.255.248 | 10.32.7.143 |

**Tabel pengaturan interface router:**

| INTERFACE FOOSHA |             |     NETMASK     |
|:----------------:|:-----------:|:---------------:|
| NAT              |             |                 |
| WATER7           | 10.32.7.145 | 255.255.255.252 |
| GUANHAO          | 10.32.7.149 | 255.255.255.252 |

| INTERFACE WATER7 |             |     NETMASK     |
|:----------------:|:-----------:|:---------------:|
| FOOSHA           | 10.32.7.146 | 255.255.255.252 |
| DORI/JIPA        | 10.32.7.129 | 255.255.255.248 |
| BLUENO           | 10.32.7.1   | 255.255.255.128 |
| CIPHER           | 10.32.0.1   | 255.255.252.0   |

| INTERFACE GUANHAO |             |     NETMASK     |
|:-----------------:|:-----------:|:---------------:|
| FOOSHA            | 10.32.7.150 | 255.255.255.252 |
| ELENA             | 10.32.4.1   | 255.255.254.0   |
| JORGE/MAIN        | 10.32.7.137 | 255.255.255.248 |
| FUKUROU           | 10.32.6.1   | 255.255.255.0   |

**Tabel pengaturan interface client & server**
| INTERFACE DORIKI |             |   GATEWAY   |     NETMASK     |
|:----------------:|:-----------:|:-----------:|:---------------:|
| WATER7           | 10.32.7.130 | 10.32.7.129 | 255.255.255.248 |

| INTERFACE JIPANGU |             |   GATEWAY   |     NETMASK     |
|:-----------------:|:-----------:|:-----------:|:---------------:|
| WATER7            | 10.32.7.131 | 10.32.7.129 | 255.255.255.248 |

| INTERFACE BLUENO |           |  GATEWAY  |     NETMASK     |
|:----------------:|:---------:|:---------:|:---------------:|
| WATER7           | 10.32.7.2 | 10.32.7.1 | 255.255.255.128 |

| INTERFACE CIPHER |           |  GATEWAY  |    NETMASK    |
|:----------------:|:---------:|:---------:|:-------------:|
| WATER7           | 10.32.0.2 | 10.32.0.1 | 255.255.252.0 |

| INTERFACE ELENA |           |  GATEWAY  |    NETMASK    |
|:---------------:|:---------:|:---------:|:-------------:|
| PUCCI           | 10.32.4.2 | 10.32.4.1 | 255.255.254.0 |

| INTERFACE FUKUROU |           |  GATEWAY  |    NETMASK    |
|:-----------------:|:---------:|:---------:|:-------------:|
| PUCCI             | 10.32.6.2 | 10.32.6.1 | 255.255.255.0 |

| INTERFACE JORGE |             |   GATEWAY   |     NETMASK     |
|:---------------:|:-----------:|:-----------:|:---------------:|
| PUCCI           | 10.32.7.138 | 10.32.7.137 | 255.255.255.248 |

| INTERFACE MAINGATE |             |   GATEWAY   |     NETMASK     |
|:------------------:|:-----------:|:-----------:|:---------------:|
| PUCCI              | 10.32.7.139 | 10.32.7.137 | 255.255.255.248 |

## (C)
### Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

**Dari tabel subnetting diatas, maka tabel routing dari topologi ini adalah sebagai berikut:**

| ROUTING FOOSHA |                 |     |             |
|:--------------:|:---------------:|:---:|:-----------:|
| 10.32.0.0      | 255.255.252.0   | via | 10.32.7.146 |
| 10.32.7.0      | 255.255.255.128 | via | 10.32.7.146 |
| 10.32.7.128    | 255.255.255.248 | via | 10.32.7.146 |
| 10.32.4.0      | 255.255.254.0   | via | 10.32.7.150 |
| 10.32.6.0      | 255.255.255.0   | via | 10.32.7.150 |
| 10.32.7.136    | 255.255.255.248 | via | 10.32.7.150 |

| ROUTING WATER7 |         |     |             |
|:--------------:|:-------:|:---:|:-----------:|
| 0.0.0.0        | 0.0.0.0 | via | 10.32.7.145 |

| ROUTING PUCCI |         |     |             |
|:-------------:|:-------:|:---:|:-----------:|
| 0.0.0.0       | 0.0.0.0 | via | 10.32.7.149 |

**Berikut adalah konfigurasi routing pada FOOSHA**
```bash

// CIPHER
route add -net 10.32.0.0 netmask 255.255.252.0 gw 10.32.7.146

// BLUENO
route add -net 10.32.7.0 netmask 255.255.255.128 gw 10.32.7.146

// DORI/JIPA
route add -net 10.32.7.128 netmask 255.255.255.248 gw 10.32.7.146

// ELENA
route add -net 10.32.4.0 netmask 255.255.254.0 gw 10.32.7.150

// FUKUROU
route add -net 10.32.6.0 netmask 255.255.255.0 gw 10.32.7.150

// JORGE/MAIN
route add -net 10.32.7.136 netmask 255.255.255.248 gw 10.32.7.150
```

**Sedangkan pada Water7 dan Pucci tidak ditambahkan routing tambahan dikarenakan sudah terdapat default routing pada router tersebut.**

## (D)
### Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

### Solusi
**Ubah Hosts Config**
```bash
#Blueno, Cipher, Elena, dan Fukurou
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

**Setting DHCP Server (Jipangu)**
Install
```bash
apt-get update
apt-get install isc-dhcp-server -y
```
Ubah config interfaces
```bash
# /etc/default/isc-dhcp-server
INTERFACES="eth0"
```
Ubah `dhcpd.conf`
```bash
# Blueno
subnet 10.32.7.0 netmask 255.255.255.128 {
	range 10.32.7.2 10.32.7.126;
	option routers 10.32.7.1;
	option broadcast-address 10.32.7.127;
	option domain-name-servers 10.32.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}

# Cipher
subnet 10.32.0.0 netmask 255.255.252.0 {
	range 10.32.0.2 10.32.3.254;
	option routers 10.32.0.1;
	option broadcast-address 10.32.3.255;
	option domain-name-servers 10.32.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}

# Elena
subnet 10.32.4.0 netmask 255.255.254.0 {
	range 10.32.4.2 10.32.5.254;
	option routers 10.32.4.1;
	option broadcast-address 10.32.5.255;
	option domain-name-servers 10.32.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}

# Fukurou
subnet 10.32.6.0 netmask 255.255.255.0 {
	range 10.32.6.2 10.32.6.254;
	option routers 10.32.6.1;
	option broadcast-address 10.32.6.255;
	option domain-name-servers 10.32.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}

# Routing dari Jipangu ke router
subnet 10.32.7.128 netmask 255.255.255.248 {
    option routers 10.32.7.129;
}
```
**Setting Relay (Water7 dan Guanhao)**
Install
```bash
apt-get update
apt-get install isc-dhcp-relay -y
```
Config
```bash
# /etc/default/isc-dhcp-relay
# What servers should the DHCP relay forward requests to?
SERVERS="10.32.7.31"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
### Test Ping

Blueno

![Untitled (1)](https://user-images.githubusercontent.com/43901559/145421611-a4647418-928b-4d15-a898-95c7f47f0415.png)

Cipher

![Untitled (2)](https://user-images.githubusercontent.com/43901559/145421654-6cb83d14-994c-4212-9c93-82402688e2d6.png)

Elena

![Untitled (3)](https://user-images.githubusercontent.com/43901559/145421697-8131c252-dcf8-464a-8d76-f06af2c3ee14.png)

![Untitled (4)](https://user-images.githubusercontent.com/43901559/145421705-e4c4decb-feae-4f07-8191-049f7b5da923.png)

Fukurou

![Untitled (5)](https://user-images.githubusercontent.com/43901559/145421738-8fc626aa-9f32-4284-967a-789b1bdeeb82.png)

## 1.
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.
```bash
iptables -t nat -A POSTROUTING -s 10.32.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.233
```
### Penjelasan:

NAT Table pada POSTROUTING chain untuk mengubah source address yang awalnya berupa private IPv4 address yang memiliki 16-bit blok dari private IP addresses yaitu 10.32.0.0/21  menjadi IP eth0 Foosha yaitu 192.168.122.233

## 2.
### Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.
```bash
iptables -A FORWARD -d 10.32.7.128/29 -i eth0 -p tcp --dport 80 -j DROP
```
### Penjelasan:

Menggunakan FORWARD chain untuk menyaring paket dengan protokol TCP dari luar topologi menuju ke DHCP Server dan DNS Server yang berada di satu subnet yang sama yaitu 10.32.7.128/29 (A1), dimana akses HTTP (Port 80) yang masuk ke DHCP Server dan DNS Server akan di drop

## 3.
### Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.
```bash
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```
### Penjelasan:

Menggunakan INPUT chain untuk menyaring paket dengan protokol ICMP yang masuk agar dibatasi hanya sebatas maksimal 3 koneksi saja menggunakan `--connlimit-above 3` kemudian bisa diakses darimana saja menggunakan `--connlimit-mask 0`, jika lebih dari 3 akan di DROP

Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut

## 4.
### Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

### Solusi

```bash
# Blueno
iptables -A INPUT -s 10.32.7.0/25 -m time --timestart 07:00 --timestop 15:00 -j ACCEPT
iptables -A INPUT -s 10.32.7.0/25 -j REJECT

# Cipher
iptables -A INPUT -s 10.32.0.0/22 -m time --timestart 07:00 --timestop 15:00 -j ACCEPT
iptables -A INPUT -s 10.32.0.0/22 -j REJECT
```

```bash
Senin
date -s "7 DEC 2021 08:00:00" -> bisa
date -s "7 DEC 2021 20:00:00" -> gabisa

Sabtu
date -s "11 DEC 2021 09:00:00" -> gabisa
date -s "11 DEC 2021 21:00:00" -> gabisa
```

## 5.
### Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
Selain itu di reject

### Solusi
Script
```bash
# Elena
iptables -A INPUT -s 10.32.4.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.32.4.0/23 -j REJECT

# Fukurou
iptables -A INPUT -s 10.32.6.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.32.6.0/24 -j REJECT
```
Testing
```bash
# Elena
iptables -A INPUT -s 10.32.4.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.32.4.0/23 -j REJECT

# Fukurou
iptables -A INPUT -s 10.32.6.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.32.6.0/24 -j REJECT
```

## 6.
### Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

### Solusi
Script di Guanhao
```bash
iptables -t nat -A PREROUTING -d 10.32.7.130 -p tcp --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.32.7.139
iptables -t nat -A PREROUTING -d 10.32.7.130 -p tcp --dport 80 -j DNAT --to-destination 10.32.7.138
```
