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

Kedua, pada node Berlint, kita akan menginstal squid karena Berlint berperan sebagai Proxy Server

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal1.sh```

* Berlint 
```
apt-get update
apt-get install squid -y
```

Ketiga, pada node Westalis, kita akan menginstal **isc-dhcp-server** karena Westalis berperan sebagai DHCP Server

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal1.sh```

* Westalis
```
apt-get update
apt-get install isc-dhcp-server -y
```

Lalu untuk node-node lainnya, kita dapat mengetes apakah masing-masing node sudah terhubung jaringan internet dengan menjalankan ```bash soal1.sh``` pada masing-masing node

```
ping google.com -c 5
```

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
*) Apabila muncul ```[fail]``` setelah me-*restart*, kita dapat menonaktifkan dan mengaktifkan **isc-dhcp-server** secara manual

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

Setelah itu, kita dapat mengetes apakah client dapat terhubung dengan internet melalui DNS dengan cara melakukan ping ke IP Address DNS Server

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal5.sh```

* Client
```
ping 10.17.2.2 -c 5
```

## Soal 6
5. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.
### Jawab
Karena kita sudah mengatur ```default-lease-time``` dan ```max-lease-time``` pada saat mengerjakan soal no. 3 dan 4, kita dapat langsung mengetesnya pada client. Tapi sebelumnya, kita harus mengonfigurasikan DHCP Client dengan cara mengganti isi file ```/etc/network/interfaces``` agar sesuai dengan script yang ada di bawah. Setelah itu, *restart* node client. Setelah berhasil me-*restart*, cek ip node client dengan ```ip a``` untuk melihat perubahan IP Address serta informasi *lease time*

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal6.sh```

* Client
```
echo -e '
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces
```

## Soal 7
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13
### Jawab
Pertama, pada node Westalis, kita dapat menambahkan konfigurasi seperti pada script di bawah pada file ```/etc/dhcp/dhcpd.conf```. Kemudian kita dapat me-*restart* **isc-dhcp-server**

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal7.sh```

* Westalis
```
echo -e '
host Eden {
    hardware ethernet 1e:d7:57:4e:c8:41;
    fixed-address 10.17.3.13;
}
' >> /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

Lalu beralih ke node Eden, kita dapat menambahkan ```hwaddress ether 1e:d7:57:4e:c8:41``` ke dalam file ```/etc/network/interfaces```

> kita dapat langsung menjalankannya dengan melakukan command ```bash soal7.sh```

* Eden 
```
echo -e '
auto eth0
iface eth0 inet dhcp
hwaddress ether 1e:d7:57:4e:c8:41
' > /etc/network/interfaces
```

Setelah menjalankan command tersebut, *restart* node Eden. Kemudian, cek ip node client dengan ```ip a``` untuk melihat perubahan IP Address
