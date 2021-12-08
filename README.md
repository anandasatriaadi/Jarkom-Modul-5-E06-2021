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

## 6.
### Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate
