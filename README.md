# Jarkom-Modul-3-E17-2023
Kelompok E17 -
Jaringan Komputer (F) </br>
*Insitut Teknologi Sepuluh Nopember*

**Authors :**
| Name                  | Student ID |
| ----------------------|------------|
| Rizky Alifiyah Rahma  | 5025211208 |
| Dilla Wahdana         | 5025211234 |

## Soal 0
Topologi
![Alt Text]()
### Configurasi:
ROUTER - AURA
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.45.1.10
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.45.2.10
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.45.3.10
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.45.4.10
	netmask 255.255.255.0
```
[Switch 1]

HIMMEL
```
auto eth0
iface eth0 inet static
	address 10.45.1.1
	netmask 255.255.255.0
	gateway 10.45.1.10
```
HEITER
```
auto eth0
iface eth0 inet static
	address 10.45.1.2
	netmask 255.255.255.0
	gateway 10.45.1.10
```
[Switch 2]

DENKEN
```
auto eth0
iface eth0 inet static
	address 10.45.2.1
	netmask 255.255.255.0
	gateway 10.45.2.10
```
EISEN
```
auto eth0
iface eth0 inet static
	address 10.45.2.2
	netmask 255.255.255.0
	gateway 10.45.2.10
```
[Switch 3]

LAWINE
```
auto eth0
iface eth0 inet static
	address 10.45.3.1
	netmask 255.255.255.0
	gateway 10.45.3.0
```
LINIE
```
auto eth0
iface eth0 inet static
	address 10.45.3.2
	netmask 255.255.255.0
	gateway 10.45.3.0

auto eth0
iface eth0 inet dhcp

LUGNER
auto eth0
iface eth0 inet static
	address 10.45.3.3
	netmask 255.255.255.0
	gateway 10.45.3.0
```
REVOITE
```
auto eth0
iface eth0 inet static
	address 10.45.3.4
	netmask 255.255.255.0
	gateway 10.45.3.0
```
RICHTER
```
auto eth0
iface eth0 inet static
	address 10.45.3.5
	netmask 255.255.255.0
	gateway 10.45.3.0
```
[Switch 4]

FRIEREN
```
auto eth0
iface eth0 inet static
	address 10.45.4.1
	netmask 255.255.255.0
	gateway 10.45.4.0
```
FLAMME
```
auto eth0
iface eth0 inet static
	address 10.45.4.2
	netmask 255.255.255.0
	gateway 10.45.4.0
```
FERN
```
auto eth0
iface eth0 inet static
	address 10.45.4.3
	netmask 255.255.255.0
	gateway 10.45.4.0
```
SEIN
```
auto eth0
iface eth0 inet static
	address 10.45.4.4
	netmask 255.255.255.0
	gateway 10.45.4.0
```

## Soal 1
> lakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.

### Script Pengerjaan
- Dilakukan setup terlebih dahulu pada Node Heiter (DNS Master) antara lain mengkoneksikan ke routernya menggunakan ``echo nameserver 192.168.122.1 > /etc/resolv.conf`` lalu Instalasi bind ``apt-get update`` dan ``apt-get install bind9 -y``
- Kemudian buat domain dengan nama ``canyon.E17.com`` dan ``channel.E17.com`` berikut sciptnya:
lakukan perintah dibawah dan isi konfigurasi domain ``canyon.E17.com`` dan ``channel.E17.com``
```
nano /etc/bind/named.conf.local
zone “canyon.E17.com” {
	type master;
	file “/etc/bind/jarkom/canyon.E17.com”;
};

zone “granz.channel.E17.com” {
	type master;
	file “/etc/bind/jarkom/channel.E17.com”;
};

zone "1.45.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/1.45.10.in-addr.arpa";
};
```
Buat folder jarkom di dalam ``/etc/bind`` dan copykan file ``db.local`` ke folder ``jarkom`` yang baru saja dibuat
```
mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/canyon.E17.com
cp /etc/bind/db.local /etc/bind/jarkom/channel.E17.com
cp /etc/bind/db.local /etc/bind/jarkom/1.45.10.in-addr.arpa
```
Kemudian buka file ``canyon.E17.com``, ``channel.E17.com``, ``1.45.10.in-addr.arpa`` dan edit syntaxnya dengan mengarah pada worker yang memiliki IP [prefix IP].x.1
```
nano /etc/bind/jarkom/canyon.E17.com
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     canyon.E17.com. root.canyon.E17.com. (
                        2      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      canyon.E17.com.
@       IN      A       10.45.1.2 	; IP Heiter
riegel  IN      A       10.45.4.1	; IP Frieren

nano /etc/bind/jarkom/channel.E17.com
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     channel.E17.com. root.channel.E17.com. (
                        2      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      channel.E17.com.
@       IN      A       10.45.1.2 	; IP Heiter
granz  IN      A       10.45.3.1	; IP Lawine

nano /etc/bind/jarkom/1.45.10.in-addr.arpa
;
; BIND data file for local loopback interface
;
$TTL	604800
@	IN	SOA	canyon.E17.com.	root.canyon.E17.com. (
				2		; Serial
			604800		; Refresh
			86400			; Retry
			2419200		; Expire
			604800		; Negative Cache TTL
;
1.45.10. In-addr.arpa.		IN	NS	canyon.E17.com.
2				IN	PTR	canyon.E17.com.
```
Pastikan konfigurasinya sudah sesuai, lalu restart bind9
```
service bind9 restart
```
- Lakukan ujicoba pada node Client dengan script berikut:
Hubungkan dengan router dan DNS Servernya
```
nano /etc/resolv.conf

Nameserver 192.168.122.1
Nameserver 10.45.1.2
```
Jalankan ``riegel.canyon.E17.com`` dan ``granz.channel.E17.com`` dengan command
```
ping riegel.canyon.E17.com
ping granz.channel.E17.com
```
### Hasil
![Alt Text]()

## Soal 2
> Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

### Script Pengerjaan
- Pada topologi ini, kita akan menjadikan Himmel sebagai DHCP Server. Oleh sebab itu, kita harus meng-install isc-dhcp-server di Himmel. sebelum melakukan instalasi lakukan update package lists di Himmel
```
apt-get update
apt-get install isc-dhcp-server
```
- Menentukan interface. interface dari Himmel yang menuju ke switch adalah eth0, maka kita akan memilih interface eth0 untuk diberikan layanan DHCP.
```
nano /etc/default/isc-dhcp-server
INTERFACESv4=”eth0”
```
- Edit file konfigurasi isc-dhcp-server pada /etc/dhcp/dhcpd.conf. Tambahkan Script Konfigurasi sebagai berikut:
```
nano /etc/dhcp/dhcpd.conf

subnet 10.45.1.0 netmask 255.255.255.0 {
}

subnet 10.45.2.0 netmask 255.255.255.0 {
}

subnet 10.45.3.0 netmask 255.255.255.0 {
    range 10.45.3.16 10.45.3.32;
    range 10.45.3.64 10.45.3.80;
    option routers 10.45.3.10;
}
host Lawine {
	hardware ethernet b2:d1:7c:b8:ff:5e;
	fixed-address 10.45.3.1;
}

host Linie {
	hardware ethernet 22:e4:a6:dc:b7:ab;
	fixed-address 10.45.3.2;
}

Host Lugner {
	hardware ethernet ea:0c:3b:a2:c7:77;
	fixed-address 10.45.3.3;
}
```
- Restart Service isc-dhcp-server Dengan Perintah
```
service isc-dhcp-server restart
service isc-dhcp-server status
```
- Perbarui configurasi dari masing-masing client dengan perintah
```
nano /etc/network/interfaces
auto eth0
iface eth0 inet dhcp
```
- Perbarui juga pada masing-masing worker dengan perintah
Lawine
```
nano /etc/network/interfaces
auto eth0
iface eth0 inet dhcp
hwaddress ether b2:d1:7c:b8:ff:5e
```
Linie
```
nano /etc/network/interfaces
auto eth0
iface eth0 inet dhcp
hwaddress ether 22:e4:a6:dc:b7:ab
```
Lugner
```
nano /etc/network/interfaces
auto eth0
iface eth0 inet dhcp
hwaddress ether ea:0c:3b:a2:c7:77
```

## Soal 3
> Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

### Script Pengerjaan
- Lanjutkan dari no.2 diatas dengan menambah file konfigurasi isc-dhcp-server pada /etc/dhcp/dhcpd.conf pada node Himmel. Tambahkan Script Konfigurasi sebagai berikut:
```
nano /etc/dhcp/dhcpd.conf
subnet 10.45.4.0 netmask 255.255.255.0 {
    range 10.45.4.12 10.45.4.32;
    range 10.45.4.160 10.45.4.168;
    option routers 10.45.4.10;
}

host Frieren {
	hardware ethernet 22:9f:e7:6d:1c:8f;
	fixed-address 10.45.4.1;
}

host Flamme {
	hardware ethernet b2:6c:77:81:3b:25;
	fixed-address 10.45.4.2;
}

host Fern {
	hardware ethernet 5a:03:2e:47:a5:5f;
	fixed-address 10.45.4.3;
}
```
- Restart Service isc-dhcp-server Dengan Perintah
```
service isc-dhcp-server restart
service isc-dhcp-server status
```

## Soal 4
> Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

### Script Pengerjaan
- Lakukan beberapa instalasi sebelum melakukan konfigurasi pada Aura yang dijadikan sebagai DHCP Relay. Instalasi yang dilakukan adalah sebagai berikut.
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start
```
- Melakukan Konfigurasi pada isc-dhcp-relay pada Aura(router) dengan script berikut:
```
nano /etc/default/isc-dhcp-relay
SERVERS=”10.45.1.1”
INTERFACES=”eth1 eth2 eth3 eth4”
OPTIONS=””
```
- Melakukan Konfigurasi IP Forwarding pada /etc/sysctl.conf
```
nano /etc/sysctl.conf
net.ipv4.ip_forward=1
```
- Kemudian, restart service isc-dhcp-relay
```
service isc-dhcp-relay restart
```

## Soal 5
> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

### Script Pengerjaan
- Lanjutkan dari no.3 diatas dengan menambah file konfigurasi isc-dhcp-server pada /etc/dhcp/dhcpd.conf pada node Himmel, tambahkan lama waktu DHCP server meminjamkan alamat IP kepada Client. Berikut Script Konfigurasinya:
```
nano /etc/dhcp/dhcpd.conf
subnet 10.45.1.0 netmask 255.255.255.0 {
}

subnet 10.45.2.0 netmask 255.255.255.0 {
}

subnet 10.45.3.0 netmask 255.255.255.0 {
    range 10.45.3.16 10.45.3.32;
    range 10.45.3.64 10.45.3.80;
    option routers 10.45.3.10;
    option broadcast-address 10.45.3.255;
    option domain-name-servers 10.45.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.45.4.0 netmask 255.255.255.0 {
    range 10.45.4.12 10.45.4.32;
    range 10.45.4.160 10.45.4.168;
    option routers 10.45.4.10;
    option broadcast-address 10.45.4.255;
    option domain-name-servers 10.45.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
```
- Restart Service isc-dhcp-server Dengan Perintah
```
service isc-dhcp-server restart
service isc-dhcp-server status
```

### Hasil
![Alt Text]()

## Soal 6
> Berjalannya waktu, petualang diminta untuk melakukan deployment. Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website ``berikut`` dengan menggunakan php 7.3

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 7
> Kepala suku dari ``Bredt Region`` memberikan resource server sebagai berikut:
a. Lawine, 4GB, 2vCPU, dan 80 GB SSD.
b. Linie, 2GB, 2vCPU, dan 50 GB SSD.
c. Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 8
> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
a. Nama Algoritma Load Balancer
b. Report hasil testing pada Apache Benchmark
c. Grafik request per second untuk masing masing algoritma. 
d. Analisis

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 9
> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 10
> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 11
> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman ``https://www.its.ac.id.``

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 12
> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 13
> Karena para petualang kehabisan uang, mereka kembali bekerja untuk mengatur riegel.canyon.yyy.com.
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 14
> Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan ``quest guide`` berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 15
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
a. POST /auth/register

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 16
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
b. POST /auth/login

### Script Pengerjaan
### Hasil
![Alt Text]()

## Sola 17
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
> c. GET /me

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 18
> Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 19
> Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
> - pm.max_children
> - pm.start_servers
> - pm.min_spare_servers
> - pm.max_spare_servers

> sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

### Script Pengerjaan
### Hasil
![Alt Text]()

## Soal 20
> Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

### Script Pengerjaan
### Hasil
![Alt Text]()
