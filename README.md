# Jarkom-Modul-3-D04-2022

## Anggota Kelompok
| **Nama** | **NRP** |
| ------ | ------ |
| Muhammad Ghani Taufiqurrahman Atmaja | 5025201110 |
| Tengku Fredly Reinaldo | 5025201198 |
| Shaula Aljauhara Riyadi | 5025201265 |

## Intro
<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200877881-903d0467-2e00-40e1-9c74-a9f271b1092c.png"> 
</p>

**Sebelum** kita mulai mengerjakan soal, buatlah beberapa node ethernet switch dan ubuntu serta hubungan antar node seperti pada gambar di atas.  Setelah itu, setting network masing-masing node dengan fitur ```Edit network configuration``` lalu hapus semua settingnya dan isi dengan settingan di bawah:

* Ostania

```
auto eth0
iface eth0 inet dhcp
auto eth1
iface eth1 inet static
	address 10.17.1.1
	netmask 255.255.255.0
auto eth2
iface eth2 inet static
	address 10.17.2.1
	netmask 255.255.255.0
auto eth3
iface eth3 inet static
	address 10.17.3.1
	netmask 255.255.255.0
```

* SSS

```
auto eth0
iface eth0 inet static
	address 10.17.1.2
	netmask 255.255.255.0
	gateway 10.17.1.1
```

* Garden

```
auto eth0
iface eth0 inet static
	address 10.17.1.3
	netmask 255.255.255.0
	gateway 10.17.1.1
```

* WISE

```
auto eth0
iface eth0 inet static
	address 10.17.2.2
	netmask 255.255.255.0
	gateway 10.17.2.1
```

* Berlint

```
auto eth0
iface eth0 inet static
	address 10.17.2.3
	netmask 255.255.255.0
	gateway 10.17.2.1
```

* Westalis

```
auto eth0
iface eth0 inet static
	address 10.17.2.3
	netmask 255.255.255.0
	gateway 10.17.2.1
```

* Eden

```
auto eth0
iface eth0 inet static
	address 10.17.3.2
	netmask 255.255.255.0
	gateway 10.17.3.1
```

* NewstonCastle

```
auto eth0
iface eth0 inet static
	address 10.17.3.3
	netmask 255.255.255.0
	gateway 10.17.3.1
```

* KemonoPark

```
auto eth0
iface eth0 inet static
	address 10.17.3.3
	netmask 255.255.255.0
	gateway 10.17.3.1
```

Setelah itu, tambahkan isi file ```root/.bashrc``` pada masing-masing node dengan command di bawah ini:

* Ostania

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.17.0.0/16
```

* Client

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
```

* WISE, Berlint, dan Westalis

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

***Semua script yang akan ditampilkan di bawah sudah disimpan ke dalam file shell script di ```/root``` masing-masing node. Yang perlu anda lakukan hanyalah menjalankan file ```.sh``` tersebut, sesuai dengan nomor yang akan dikerjakan.

## Soal 1
Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server
## Jawab
Pertama, pada node WISE, kita akan menginstal bind9 karena WISE berperan sebagai DNS Server

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal1.sh```

* WISE 
```
apt-get update 
apt-get install bind9 -y
```
<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200989410-e8a4e63e-971f-4cae-b661-6ff996274771.jpeg"> 
</p>

Kedua, pada node Berlint, kita akan menginstal squid karena Berlint berperan sebagai Proxy Server

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal1.sh```

* Berlint 
```
apt-get update
apt-get install squid -y
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200990983-970a0d65-130f-4133-b1b9-c4cf46964aa2.jpeg"> 
</p>

Ketiga, pada node Westalis, kita akan menginstal **isc-dhcp-server** karena Westalis berperan sebagai DHCP Server

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal1.sh```

* Westalis
```
apt-get update
apt-get install isc-dhcp-server -y
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200991025-5d91ef84-ae65-4bd1-a40c-ed0eea7f71f8.jpeg"> 
</p>

Lalu untuk node-node lainnya, kita dapat mengetes apakah masing-masing node sudah terhubung jaringan internet dengan menjalankan ```bash soal1.sh``` pada masing-masing node

* Ostania dan Client

```
ping google.com -c 5
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200991239-9912abcf-b02d-4d77-88d8-655163b0fd4e.jpeg"> 
</p>
<p float="left" align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200991463-ff238827-2ddb-4646-87ed-688c77794987.jpg" width=48% height=48%> 
  <img src="https://user-images.githubusercontent.com/56400536/200991494-2c45f047-1cb1-4494-8a9b-27f276497412.jpg" width=48% height=48%>
  <img src="https://user-images.githubusercontent.com/56400536/200991703-691edd71-876c-4609-8791-e638db6c4e73.jpg" width=48% height=48%> 
  <img src="https://user-images.githubusercontent.com/56400536/200991520-f85966ab-a832-4eb9-8c0c-db794c0a3d3e.jpg" width=48% height=48%> 
  <img src="https://user-images.githubusercontent.com/56400536/200991554-a7ab0236-5a75-47af-ac8e-4b06a00c1afa.jpg" width=48% height=48%>
</p>

## Soal 2
, dan Ostania sebagai DHCP Relay
### Jawab
Pertama, pada node Ostania, kita akan menginstal **isc-dhcp-relay** karena Ostania berperan sebagai DHCP Relay. Kemudian dalam proses instalasi, isi bagian ```DHCP relay should forward requests to:``` dengan ```10.17.2.4``` (IP Address DHCP Server) dan isi juga bagian ```DHCP relay should listen on:``` dengan ```eth1 eth2 eth3```. Setelah terinstal, kita akan me-*restart* **isc-dhcp-relay** 

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal2.sh```

* Ostania
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay restart
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200992287-6c0fc18d-9e59-4ebc-ae2e-81c742d056bb.jpeg"> 
</p>

## Soal 3
Ada beberapa kriteria yang ingin dibuat oleh Loid dan Franky, yaitu:
1. Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server
2. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155
### Jawab
Pertama, pada node Westalis, kita harus mengatur ```INTERFACES``` terlebih dahulu di dalam file ```/etc/default/isc-dhcp-server```. Kemudian menambah konfigurasi di dalam file ```/etc/dhcp/dhcpd.conf``` sesuai dengan script yang ada di bawah. Setelah itu, kita akan me-*restart* **isc-dhcp-server** 

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal3.sh```

* Westalis
```
echo -e '
INTERFACES="eth0"
' > /etc/default/isc-dhcp-server

echo -e '
subnet 10.17.1.0 netmask 255.255.255.0 {
    range 10.17.1.50 10.17.1.88;
    range 10.17.1.120 10.17.1.155;
    option routers 10.17.1.1;
    option broadcast-address 10.17.1.255;
    option domain-name-servers 10.17.2.2;
    default-lease-time 300;
    max-lease-time 6900;
}

subnet 10.17.2.0 netmask 255.255.255.0 {
    option routers 10.17.2.1;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200992400-3a528335-7ad4-4780-a2db-53010475efc2.jpg"> 
</p>

*) Apabila muncul ```[fail]``` seperti pada gambar di atas, kita dapat menonaktifkan dan mengaktifkan **isc-dhcp-server** secara manual

## Soal 4
3. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85
### Jawab
Pada node Westalis, kita dapat menambahkan konfigurasi yang ada pada script di bawah ke dalam file ```/etc/dhcp/dhcpd.conf```. Kemudian kita akan me-*restart* **isc-dhcp-server** 

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal4.sh```

* Westalis
```
echo -e '
subnet 10.17.3.0 netmask 255.255.255.0 {
    range 10.17.3.10 10.17.3.30;
    range 10.17.3.60 10.17.3.85;
    option routers 10.17.3.1;
    option broadcast-address 10.17.3.255;
    option domain-name-servers 10.17.2.2;
    default-lease-time 600;
    max-lease-time 6900;
}
' >> /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200992521-7a932472-713e-47c2-b5c5-b80b9690ff44.jpg"> 
</p>

## Soal 5
4. Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.
## Jawab
Pertama, pada node WISE, kita akan mengonfigurasikan file ```/etc/bind/named.conf.options``` agar sesuai dengan script di bawah. Kemudian kita dapat me-*restart* bind9

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal5.sh```

* WISE
```
echo -e '
options {
        directory "/var/cache/bind";

        forwarders {
             192.168.122.1;
        };

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options

service bind9 restart
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200992597-9e4f55b2-79c1-45c5-a769-c40f4cebb2b8.jpg"> 
</p>

Setelah itu, kita dapat mengetes apakah client dapat terhubung dengan internet melalui DNS dengan cara melakukan ping ke IP Address DNS Server

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal5.sh```

* Client
```
ping 10.17.2.2 -c 5
```

<p float="left" align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200992671-9a278115-54d4-4cc4-bd3c-8a3de2cf7dd6.jpg" width=48% height=48%> 
  <img src="https://user-images.githubusercontent.com/56400536/200992700-eddf6194-3d5e-4e14-8bf7-273c88a95dee.jpg" width=48% height=48%>
  <img src="https://user-images.githubusercontent.com/56400536/200992733-3bccb14f-8978-404f-a2b1-3dd593f47a26.jpg" width=48% height=48%> 
  <img src="https://user-images.githubusercontent.com/56400536/200992756-c7056405-3bf1-4011-87a1-ec5195fb4767.jpg" width=48% height=48%>
  <img src="https://user-images.githubusercontent.com/56400536/200992783-01167770-fa55-4eb0-882e-81a57c1d0f47.jpg" width=48% height=48%> 
</p>

## Soal 6
5. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.
### Jawab
Karena kita sudah mengatur ```default-lease-time``` dan ```max-lease-time``` pada saat mengerjakan soal no. 3 dan 4, kita dapat langsung mengetesnya pada client. Tapi sebelumnya, kita harus mengonfigurasikan DHCP Client dengan cara mengganti isi file ```/etc/network/interfaces``` agar sesuai dengan script yang ada di bawah.

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal6.sh```

* Client
```
echo -e '
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces
```

Setelah menjalankan command tersebut, *restart* node client. Kemudian, cek IP node client dengan ```ip a``` untuk melihat perubahan IP Address

<p float="left" align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200993194-d4cc76f2-2de3-4980-84b2-a44184221f8c.jpg" width=48% height=48%> 
  <img src="https://user-images.githubusercontent.com/56400536/200993165-ee2497e9-5614-4ce6-a34a-f65b93bdb540.jpg" width=48% height=48%>
  <img src="https://user-images.githubusercontent.com/56400536/200993074-cdb1e475-3cf6-4c01-ae65-f1955ea57d99.jpg" width=48% height=48%> 
  <img src="https://user-images.githubusercontent.com/56400536/200993011-5e26ab27-5294-4aa5-882b-0b47de6dfdb3.jpg" width=48% height=48%>
  <img src="https://user-images.githubusercontent.com/56400536/200992999-c772ac88-1f14-4ec2-9257-3fa83009620f.jpg" width=48% height=48%> 
</p>

## Soal 7
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13
### Jawab
Pertama, pada node Westalis, kita dapat menambahkan konfigurasi seperti pada script di bawah pada file ```/etc/dhcp/dhcpd.conf```. Kemudian kita dapat me-*restart* **isc-dhcp-server**

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal7.sh```

* Westalis
```
echo -e '
host Eden {
    hardware ethernet 'hwaddress_milik_Eden';
    fixed-address 10.17.3.13;
}
' >> /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200993286-a9e1146d-8153-47b0-9f08-efe22b0e8411.jpg"> 
</p>

Lalu beralih ke node Eden, kita dapat menambahkan ```hwaddress ether 'hwaddress_milik_Eden'``` ke dalam file ```/etc/network/interfaces```

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal7.sh```

* Eden 
```
echo -e '
hwaddress ether 'hwaddress_milik_Eden'
' >> /etc/network/interfaces
```

Setelah menjalankan command tersebut, *restart* node Eden. Kemudian, cek IP node Eden dengan ```ip a``` untuk melihat perubahan IP Address

<p align="center">
  <img src="https://user-images.githubusercontent.com/56400536/200994549-3e2ab548-1b53-4be7-9b57-693a112d5209.jpeg"> 
</p>

## Soal 8
Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh)
### Berlint
Dipastikan auto update waktu dan tanggal mati agar dapat mengubah tanggal dan waktu pada terminal.
```
echo '
acl WORKING time MTWHF 08:00-17:00
' > /etc/squid/acl.conf
echo '
include /etc/squid/acl.conf
http_port 8080
visible_hostname Berlint
http_access deny WORKING
http_access allow all
' > /etc/squid/squid.conf
service squid restart
```
![image](https://user-images.githubusercontent.com/77779184/201885987-22a2a2ad-5ea9-4d80-b32d-b1de01378377.png)
### SSS
```
apt-get update
apt-get install lynx -y
export http_proxy="http://10.17.2.3:8080"
date -s "7 NOV 2022 13:30:00"
lynx google.com
```
![image2](https://user-images.githubusercontent.com/77779184/201886942-d67a7805-4054-4ec9-a250-533bc7b8ea41.png)
### Garden
```
apt-get update
apt-get install lynx -y
export http_proxy="http://10.17.2.3:8080"
date -s "7 NOV 2022 18:30:00"
lynx google.com
```
![image3](https://user-images.githubusercontent.com/77779184/201887261-dd88d3f4-821c-4cc7-9c30-7f18ea807ffd.png)
## Soal 9
Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)
### WISE
```
echo '
zone "loid-work.com" {
        type master;
        file "/etc/bind/jarkom/loid-work.com";
};
zone "franky-work.com" {
        type master;
        file "/etc/bind/jarkom/franky-work.com";
};
' > /etc/bind/named.conf.local
mkdir /etc/bind/jarkom
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     loid-work.com. root.loid-work.com. (
                     2022110901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      loid-work.com.
@       IN      A       10.17.2.2
' > /etc/bind/jarkom/loid-work.com
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky-work.com. root.franky-work.com. (
                     2022110902         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky-work.com.
@       IN      A       10.17.2.2
' > /etc/bind/jarkom/franky-work.com
service bind9 restart
```
![image5](https://user-images.githubusercontent.com/77779184/201888298-0c0da15a-07b0-479c-9872-d7b2470c0fd4.png)
### Berlint
```
echo '
loid-work.com
franky-work.com
' > /etc/squid/work-sites.acl
echo '
include /etc/squid/acl.conf
http_port 8080
visible_hostname Berlint
acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
http_access allow WORKSITES
http_access deny WORKING
http_access allow all
' > /etc/squid/squid.conf
service squid restart
```
![image6](https://user-images.githubusercontent.com/77779184/201888408-fa9735a2-764d-4271-bbee-7e0456446697.png)
### SSS
```
export http_proxy="http://10.17.2.3:8080"
date -s "7 NOV 2022 13:30:00"
lynx franky-work.com
lynx loid-work.com
lynx google.com
```
> internet tidak bisa diakses
![image7](https://user-images.githubusercontent.com/77779184/201888599-9e21094a-8cb8-405f-9c18-b71ccdeeb238.png)
## Soal 10
Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)
### Berlint
```
echo '
include /etc/squid/acl.conf
http_port 8080
visible_hostname Berlint
acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
http_access allow WORKSITES
http_access deny WORKING
http_access deny all
' > /etc/squid/squid.conf
service squid restart
```
![image8](https://user-images.githubusercontent.com/77779184/201888732-a8d17161-49d6-4c0e-9e0f-2f0f69084af2.png)
### SSS
```
export http_proxy="http://10.17.2.3:8080"
date -s "7 NOV 2022 18:30:00"
lynx http://www.example.com
lynx https://www.example.com
```
> http://example.com tidak bisa diakses
![image9](https://user-images.githubusercontent.com/77779184/201888897-2981ee97-5661-4d75-838f-0eb2b5f97032.png)
## Soal 11
Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps)
### Berlint
```
echo '
include /etc/squid/acl.conf
http_port 8080
visible_hostname Berlint
acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
http_access allow WORKSITES
http_access deny WORKING
http_access allow all
delay_pools 1
delay_class 1 2 
delay_access 1 allow all
delay_parameters 1 none 16000/32000
' > /etc/squid/squid.conf
service squid restart
```
![image11](https://user-images.githubusercontent.com/77779184/201889249-ac822fa3-7d12-48c3-abe0-ad3ca9a6a795.png)
### SSS
```
unset http_proxy
apt-get install speedtest-cli -y
export PYTHONHTTPSVERIFY=0
export http_proxy="http://10.17.2.3:8080"
date -s "7 NOV 2022 18:30:00"
speedtest
```
![image12](https://user-images.githubusercontent.com/77779184/201889397-bcf6f09a-cd3d-4a30-8d81-1a6c8933db5f.png)
### Garden
```
unset http_proxy
apt-get install speedtest-cli -y
export PYTHONHTTPSVERIFY=0
export http_proxy="http://10.17.2.3:8080"
date -s "7 NOV 2022 18:30:00"
speedtest
```
![image13](https://user-images.githubusercontent.com/77779184/201889524-b3e88070-955d-4665-9cbd-82a246703d6f.png)
## Soal 12
Setelah diterapkan, ternyata peraturan nomor (4) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur
### Berlint
```
echo '
include /etc/squid/acl.conf
http_port 8080
visible_hostname Berlint
acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
http_access allow WORKSITES
http_access deny WORKING
http_access allow all
acl OPEN_TIME time MTWHF
delay_pools 1
delay_class 1 2
delay_access 1 allow !OPEN_TIME
delay_parameters 1 none 16000/32000
' > /etc/squid/squid.conf
service squid restart
```
![image14](https://user-images.githubusercontent.com/77779184/201889630-4ed87efe-10ee-43b7-8de9-4120d11f722e.png)
### SSS
```
unset http_proxy
export PYTHONHTTPSVERIFY=0
export http_proxy="http://10.17.2.3:8080"
date -s "6 NOV 2022 18:30:00"
speedtest
```
![image15](https://user-images.githubusercontent.com/77779184/201889845-52b4df79-1246-423d-9207-66a7071b17ef.png)
### Garden
```
unset http_proxy
export PYTHONHTTPSVERIFY=0
export http_proxy="http://10.17.2.3:8080"
date -s "7 NOV 2022 13:30:00"
speedtest
```
![image16](https://user-images.githubusercontent.com/77779184/201890055-9b7feaf5-0ca7-4a40-96a5-7bded26164b6.png)

## Kendala Yang Dialami
Salah satu kendala yang dialami adalah kendala ketika mengonfigurasikan DHCP Relay karena tidak termasuk di dalam materi modul
