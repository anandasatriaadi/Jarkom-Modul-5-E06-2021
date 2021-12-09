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

## (C)
### Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

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
