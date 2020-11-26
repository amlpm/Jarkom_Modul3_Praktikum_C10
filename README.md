# Jarkom_Modul3_Praktikum_C10

### Kelompok C_10
#### Asisten Dosen Bella Septina 
1. Adrian Danindra 05111840000068
2. Amelia Puji     05111840000147 
#

### Pengerjaan Soal
1. [Nomor 1](#Nomor1)
2. [Nomor 2](#Nomor2)
3. [Nomor 3](#Nomor3)
4. [Nomor 4](#Nomor4)
5. [Nomor 5](#Nomor5)
6. [Nomor 6](#Nomor6)
7. [Nomor 7](#Nomor7)
8. [Nomor 8](#Nomor8)
9. [Nomor 9](#Nomor9)
10. [Nomor 10](#Nomor10)
11. [Nomor 11](#Nomor11)
12. [Nomor 12](#Nomor12)
#

### Nomor1
#### Membuat topologi jaringan
#

Jawab : <br />
Topologi jaringan kami adalah sebagai berikut :
```
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.76.45 eth1=daemon,,,switch1 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem=256M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

# Klien
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &
xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
```

![Topologi](https://user-images.githubusercontent.com/57977401/100354452-2e183f00-302b-11eb-9f93-d54a4843181a.png)

Kemudian setelah masuk ke UML, pada router SURABAYA lakukan setting sysctl dengan mengetikkan perintah ```nano /etc/sysctl.conf```. Hilangkan tanda pagar (#) pada bagian ```net.ipv4.ip_forward=1```. Lalu mengetikkan ```sysctl -p``` untuk mengaktifkan perubahan yang ada. 

Lalu dilakukan setting IP pada setiap UML dengan mengetikkan ```nano /etc/network/interfaces``` Lalu setting sebagai berikut:

**SURABAYA (Sebagai Router)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.76.46
netmask 255.255.255.252
gateway 10.151.76.45

auto eth1
iface eth1 inet static
address 192.168.0.1
netmask 255.255.255.0

auto eth2
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.0

auto eth3
iface eth3 inet static
address 10.151.77.89
netmask 255.255.255.248
```

![Interface Surabaya](https://user-images.githubusercontent.com/57977401/100354607-5ef87400-302b-11eb-9839-13acdb5884b4.png)

**MALANG (Sebagai DNS Server Master)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.90
netmask 255.255.255.248
gateway 10.151.77.89
```

![Interface Malang](https://user-images.githubusercontent.com/57977401/100354600-5c961a00-302b-11eb-9a3e-0bd0ed8ab61e.png)

**MOJOKERTO (Sebagai Proxy Server)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.91
netmask 255.255.255.248
gateway 10.151.77.89
```

![Interfcae Mojokerto](https://user-images.githubusercontent.com/57977401/100354628-63bd2800-302b-11eb-8f68-8095a8217272.png)

**TUBAN (Sebagai DHCP Server)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.92
netmask 255.255.255.248
gateway 10.151.77.89
```

![Interface Tuban](https://user-images.githubusercontent.com/57977401/100354609-6029a100-302b-11eb-8ab2-37e2e8861c98.png)

**SIDOARJO (Sebagai Klien)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

![Interface Sidoarjo](https://user-images.githubusercontent.com/57977401/100354603-5dc74700-302b-11eb-8bdd-2d267cdb130b.png)

**GRESIK (Sebagai Klien)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

![Interface Gresik](https://user-images.githubusercontent.com/57977401/100354591-5a33c000-302b-11eb-9f68-102dee3fd5c6.png)

**BANYUWANGI (Sebagai Klien)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

![Interface Banyuwangi](https://user-images.githubusercontent.com/57977401/100354590-599b2980-302b-11eb-8c78-e9ce27240dd5.png)

**MADIUN (Sebagai Klien)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

![Interface Madiun](https://user-images.githubusercontent.com/57977401/100354596-5b64ed00-302b-11eb-922d-a77fd2ba00a9.png)

Kemudian restart network dengan mengetikkan ```service networking restart``` di setiap UML.

Terakhir, untuk melakukan perintah ```apt-get update``` pada setiap UML, lakukan langkah-langkah berikut : <br />
- Ketikkan ```nano /etc/apt/sources.list```
- Komen semua kambing.ui dan ganti dengan deb http://boyo.its.ac.id/debian stretch main contrib non-free. Save konfigurasi
- Ketikkan ```sed -i 's/buster/stretch/g' /etc/apt/sources.list``` pada tiap UML
<br />
Setelah itu, lakukan ```apt-get update```, dan siap mengerjakan soal selanjutnya

<br /><br /><br />

### Nomor2
#### Bu Meguri memerintahkan Anri untuk menjadikan SURABAYA sebagai router, MALANG sebagai DNS Server, TUBAN sebagai DHCP server, serta MOJOKERTO sebagai Proxy server, dan UML lainnya sebagai client. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.
#

Jawab : <br />
**Pada UML TUBAN, lakukan** <br />
```apt-get install isc-dhcp-server```

**Setting DHCP server (TUBAN)**
1. Ketikkan ```nano /etc/default/isc-dhcp-server```
2. Masukkan INTERFACES="eth0"

![DHCP Tuban](https://user-images.githubusercontent.com/57977401/100355903-7df80580-302d-11eb-8415-a0519953148f.png)

**Pada UML SURABAYA, lakukan** <br />
```apt-get install isc-dhcp-relay```

**SETTING DHCP RELAY (SURABAYA)**
1. ```apt-get update```
2. ```apt-get install isc-dhcp-relay```
3. Masukkan IP TUBAN yaitu ```10.151.77.93``` lalu tekan enter
4. Masukkan ```eth1 eth2 eth3``` lalu tekan enter
5. Untuk Options, tidak usah diisikan apa-apa, lalu tekan enter
6. Cek sudah sesuai apa belum dengan mengetikkan perintah ```nano /etc/default/isc-dhcp-relay```

![DHCP Surabaya](https://user-images.githubusercontent.com/57977401/100355897-7cc6d880-302d-11eb-95b8-07a6c40cce6d.png)

**Pada UML MOJOKERTO, lakukan**<br />
```apt-get install squid``` dan ```apt-get install apache2-utils```

**SETTING SQUID (MOJOKERTO)**
1. Cek status squid dengan ```service squid status```
2. Buat backup Squid dengan mengetikkan ```mv /etc/squid/squid.conf /etc/squid/squid.conf.bak```
3. Buka file squid dengan mengetikkan ``` nano /etc/squid/squid.conf```
4. Tambahkan konfigurasi berikut dan save
```
http_port 8080
visible_hostname mojokerto

http_access allow all
```
5. Lakukan ```service squid restart```

![Squid Mojokerto](https://user-images.githubusercontent.com/57977401/100373127-3ed5ae80-3045-11eb-956c-534adde5091e.png)
<br />

**Pada UML MALANG, lakukan**<br />
```apt-get install bind9```

<br /><br /><br />

### Nomor3
#### Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
#

Jawab : <br />
**Pada UML TUBAN**
1. Buka file DHCP dengan mengetikkan ```nano /etc/dhcp/dhcpd.conf```
2. Masukkan konfigurasi berikut dan simpan
```
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.77.90, 10.151.36.7, 202.46.129.2;
    default-lease-time 300;
    max-lease-time 300;
}
```
3. Lakukan ```service isc-dhcp-server restart```

![DHCP CONF](https://user-images.githubusercontent.com/57977401/100371999-9a06a180-3043-11eb-89cd-5de3c102dabf.png)

**TESTING**
1. Pada klien subnet 1, UML GRESIK dan UML SIDOARJO ketikkan perintah ```service networking restart```
2. Apabila hasilnya seperti dua gambar dibawah, maka konfigurasi berhasil

![Test Gresik](https://user-images.githubusercontent.com/57977401/100374822-ce7c5c80-3047-11eb-9c46-c48801efd4cb.png)
![Test Sidoarjo](https://user-images.githubusercontent.com/57977401/100374826-cf14f300-3047-11eb-9563-cd5e21498cbb.png)

<br /><br /><br />

### Nomor4
#### Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.
#

Jawab : <br />
**Pada UML TUBAN**
1. Buka file DHCP dengan mengetikkan ```nano /etc/dhcp/dhcpd.conf```
2. Tambahkan konfigurasi berikut dan simpan
```
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.0.50 192.168.0.70;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 10.151.77.90, 10.151.36.7, 202.46.129.2;
    default-lease-time 600;
    max-lease-time 600;
}
```
3. Lakukan ```service isc-dhcp-server restart```

![DHCP CONF](https://user-images.githubusercontent.com/57977401/100372072-b1458f00-3043-11eb-9977-b893a9cebfd0.png)

**TESTING**
1. Pada klien subnet 2, UML BANYUWANGI dan UML MADIUN ketikkan perintah ```service networking restart```
2. Apabila hasilnya seperti dua gambar dibawah, maka konfigurasi berhasil

![Test Banyuwangi](https://user-images.githubusercontent.com/57977401/100375185-5e220b00-3048-11eb-87fd-b314fba145a8.png)
![Test Madiun](https://user-images.githubusercontent.com/57977401/100375190-5f533800-3048-11eb-9c98-1534670b1463.png)

<br /><br /><br />

### Nomor5
#### Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP.
#

Jawab : <br />
**Pada UML TUBAN**
1. Buka file DHCP dengan mengetikkan ```nano /etc/dhcp/dhcpd.conf```
2. Tambahkan konfigurasi berikut dan simpan
```
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.77.90, 10.151.36.7, 202.46.129.2;
    default-lease-time 300;
    max-lease-time 300;
}

subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.0.50 192.168.0.70;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 10.151.77.90, 10.151.36.7, 202.46.129.2;
    default-lease-time 600;
    max-lease-time 600;
}
```
3. Lakukan ```service isc-dhcp-server restart```

![DHCP CONF](https://user-images.githubusercontent.com/57977401/100372072-b1458f00-3043-11eb-9977-b893a9cebfd0.png)

**TESTING**
1. Pada klien subnet 1, UML GRESIK dan UML SIDOARJO ketikkan perintah ```service networking restart```
2. Apabila hasilnya seperti dua gambar dibawah, maka konfigurasi berhasil

![Test Gresik](https://user-images.githubusercontent.com/57977401/100374822-ce7c5c80-3047-11eb-9c46-c48801efd4cb.png)
![Test Sidoarjo](https://user-images.githubusercontent.com/57977401/100374826-cf14f300-3047-11eb-9563-cd5e21498cbb.png)

3. Pada klien subnet 2, UML BANYUWANGI dan UML MADIUN ketikkan perintah ```service networking restart```
4. Apabila hasilnya seperti dua gambar dibawah, maka konfigurasi berhasil

![Test Banyuwangi](https://user-images.githubusercontent.com/57977401/100375185-5e220b00-3048-11eb-87fd-b314fba145a8.png)
![Test Madiun](https://user-images.githubusercontent.com/57977401/100375190-5f533800-3048-11eb-9c98-1534670b1463.png)

<br /><br /><br />

### Nomor6
#### Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.
#

Jawab : <br />
**Pada UML TUBAN**
1. Buka file DHCP dengan mengetikkan ```nano /etc/dhcp/dhcpd.conf```
2. Tambahkan konfigurasi berikut dan simpan
```
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.77.90, 10.151.36.7, 202.46.129.2;
    default-lease-time 300;
    max-lease-time 300;
}

subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.0.50 192.168.0.70;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 10.151.77.90, 10.151.36.7, 202.46.129.2;
    default-lease-time 600;
    max-lease-time 600;
}
```
3. Lakukan ```service isc-dhcp-server restart```

![DHCP CONF](https://user-images.githubusercontent.com/57977401/100372072-b1458f00-3043-11eb-9977-b893a9cebfd0.png)

**TESTING**
1. Pada klien subnet 1, UML GRESIK dan UML SIDOARJO ketikkan perintah ```service networking restart```
2. Apabila hasilnya seperti dua gambar dibawah, maka konfigurasi berhasil

![Test Gresik](https://user-images.githubusercontent.com/57977401/100374822-ce7c5c80-3047-11eb-9c46-c48801efd4cb.png)
![Test Sidoarjo](https://user-images.githubusercontent.com/57977401/100374826-cf14f300-3047-11eb-9563-cd5e21498cbb.png)

3. Pada klien subnet 2, UML BANYUWANGI dan UML MADIUN ketikkan perintah ```service networking restart```
4. Apabila hasilnya seperti dua gambar dibawah, maka konfigurasi berhasil

![Test Banyuwangi](https://user-images.githubusercontent.com/57977401/100375185-5e220b00-3048-11eb-87fd-b314fba145a8.png)
![Test Madiun](https://user-images.githubusercontent.com/57977401/100375190-5f533800-3048-11eb-9c98-1534670b1463.png)

<br /><br /><br />

### Nomor7
#### User autentikasi milik Anri memiliki format:
- User : userta_yyy
- Password : inipassw0rdta_yyy
#

Jawab : <br />
**Pada UML MOJOKERTO**
1. Membuat authentifikasi dengan command ```htpasswd -c /etc/squid/passwd userta_c10```

![File Passwd](https://user-images.githubusercontent.com/57977401/100373043-16e64b00-3045-11eb-8278-4cd3eed75ba7.png)

2. Buka file squid dengan mengetikkan ```nano /etc/squid/squid.conf```
3. Tambahkan konfigurasi berikut dan simpan 
```
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```
4. Lakukan ```service squid restart```

![Nomor 7](https://user-images.githubusercontent.com/57977401/100372596-71cb7280-3044-11eb-9aee-57fba1f31dcf.png)

**TESTING**
1. Pada Mozilla Firefox, carilah pengaturan proxy dan masukkan IP MOJOKRTO (10.151.77.91) dan Port 8080

![Setting Proxy](https://user-images.githubusercontent.com/57977401/100375622-0e900f00-3049-11eb-9d75-53fbe99771ec.png)

2. Apabila muncul permintaan authentifikasi seperti dibawah, maka konfigurasi sudah benar

![Nomor 7 Test](https://user-images.githubusercontent.com/57977401/100375828-62025d00-3049-11eb-8272-6696e8651626.png)

3. Coba masuk ke situs google.com. Apabila muncul permintaan authentifikasi seperti dibawah, maka konfigurasi sudah benar

![Nomor 7 Test Google com](https://user-images.githubusercontent.com/57977401/100375833-629af380-3049-11eb-962e-b0bf555b9849.png)

<br /><br /><br />

### Nomor8
#### Setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja.
#

Jawab : <br />
**Pada UML MOJOKERTO**
1. Buka file acl dengan mengetikkan ```nano /etc/squid/acl.conf```
2. Tambahkan konfigurasi berikut dan simpan 
```
acl AVAILABLE_WORKING time MTWHF 08:00-17:00
```

![Nomor 8 acl](https://user-images.githubusercontent.com/57977401/100372654-87d93300-3044-11eb-8d64-d6ae28e969b9.png)

3. Buka file squid dengan mengetikkan ```nano /etc/squid/squid.conf```
4. Tambahkan konfigurasi berikut dan simpan 
```
include /etc/squid/acl.conf

http_port 8080
http_access allow AVAILABLE_WORKING
http_access deny all
visible_hostname mojokerto
```
5. Lakukan ```service squid restart```

![Nomor 8](https://user-images.githubusercontent.com/57977401/100372659-890a6000-3044-11eb-94eb-8e28078077be.png)

**TESTING**
1. Pada Mozilla Firefox, carilah pengaturan proxy dan masukkan IP MOJOKRTO (10.151.77.91) dan Port 8080

![Setting Proxy](https://user-images.githubusercontent.com/57977401/100375622-0e900f00-3049-11eb-9d75-53fbe99771ec.png)

2.Coba masuk ke situs its.ac.id. Apabila muncul error seperti dibawah, maka konfigurasi sudah benar

![Nomor 8 test](https://user-images.githubusercontent.com/57977401/100375937-92e29200-3049-11eb-8727-05b4ddf513e5.png)

<br /><br /><br />

### Nomor9
#### Setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). Agar Anri bisa fokus mengerjakan TA,
#

Jawab : <br />
**Pada UML MOJOKERTO**
1. Buka file acl dengan mengetikkan ```nano /etc/squid/acl.conf```
2. Tambahkan konfigurasi berikut dan simpan 
```
acl DEADLINE1 time TWH 21:00-24:00
acl DEADLINE2 time WHF 00:00-09:00
```

![Nomor 9 acl](https://user-images.githubusercontent.com/57977401/100373193-5d3baa00-3045-11eb-9660-0882002f5f36.png)

3. Buka file squid dengan mengetikkan ```nano /etc/squid/squid.conf```
4. Tambahkan konfigurasi berikut dan simpan 
```
http_access allow DEADLINE1
http_access allow DEADLINE2
```
5. Lakukan ```service squid restart```

![Nomor 9](https://user-images.githubusercontent.com/57977401/100373191-5ad95000-3045-11eb-8037-4c13b064994b.png)

**TESTING**
1. Pada Mozilla Firefox, carilah pengaturan proxy dan masukkan IP MOJOKRTO (10.151.77.91) dan Port 8080

![Setting Proxy](https://user-images.githubusercontent.com/57977401/100375622-0e900f00-3049-11eb-9d75-53fbe99771ec.png)

2.Coba masuk ke situs its.ac.id. Apabila muncul error seperti dibawah, maka konfigurasi sudah benar

![Nomor 8 test](https://user-images.githubusercontent.com/57977401/100375937-92e29200-3049-11eb-8727-05b4ddf513e5.png)

<br /><br /><br />

### Nomor10
#### Setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.
#

Jawab : <br />
**Pada UML MOJOKERTO**
1. Buat file untuk menyimpan daftar situs yang di blokir (tidak boleh dikunjungi) dengan comman ```nano /etc/squid/restrict-sites.acl```
2. Tambahkan konfigurasi berikut dan simpan 
```
www.google.com
```

![Restrict-sites](https://user-images.githubusercontent.com/57977401/100373687-1e5a2400-3046-11eb-938a-ad5603944580.png)

3. Buka file squid dengan mengetikkan ```nano /etc/squid/squid.conf```
4. Tambahkan konfigurasi berikut dan simpan 
```
acl blok-situs dstdomain "/etc/squid/restrict-sites.acl"
deny_info http://monta.if.its.ac.is/index.php/berita/lihatBerita blok-situs
http_access deny blok-situs
http_access deny all
```
5. Lakukan ```service squid restart```

![Nomor 10](https://user-images.githubusercontent.com/57977401/100373685-1dc18d80-3046-11eb-8cd2-d1ecf727f384.png)

**TESTING**
1. Pada Mozilla Firefox, carilah pengaturan proxy dan masukkan IP MOJOKRTO (10.151.77.91) dan Port 8080

![Setting Proxy](https://user-images.githubusercontent.com/57977401/100375622-0e900f00-3049-11eb-9d75-53fbe99771ec.png)

2. Coba masuk ke situs google.com. Apabila yang muncul adalah monta.if.its.ac.id, maka konfigurasi sudah benar.

![Nomor 10 test](https://user-images.githubusercontent.com/57977401/100376092-df2dd200-3049-11eb-88d4-6ab46476ea15.png)

<br /><br /><br />

### Nomor11
#### Bu Meguri meminta Anri untuk mengubah error page default squid
#

Jawab : <br />
**Pada UML MOJOKERTO**
1. Buka direktori Error English dengan command ```/usr/share/squid/errors/English```
2. Rename file error yang lama dari ERR_ACCESS_DENIED menjadi ERR_ACCESS_DENIDE dengan command ```mv /usr/share/squid/errors/English/ERR_ACCESS_DENIED /usr/share/squid/errors/English/ERR_ACCESS_DENIDE```
3. Download file error dengan command ```wget 10.151.36.202/ERR_ACCESS_DENIED pada UML MOJOKERTO```
3. Lakukan ```service squid restart```

![ERR_ACCESS_DENIED](https://user-images.githubusercontent.com/57977401/100374065-b1935980-3046-11eb-9e5e-fbcd9816cdd5.png)

![Mojokerto Error](https://user-images.githubusercontent.com/57977401/100374076-b3f5b380-3046-11eb-877b-3f08eed713d0.png)

**TESTING**
1. Pada Mozilla Firefox, carilah pengaturan proxy dan masukkan IP MOJOKRTO (10.151.77.91) dan Port 8080

![Setting Proxy](https://user-images.githubusercontent.com/57977401/100375622-0e900f00-3049-11eb-9d75-53fbe99771ec.png)

2. Coba masuk ke situs its.ac.id. Apabila muncul error seperti dibawah, maka konfigurasi sudah benar

![Nomor 8 test](https://user-images.githubusercontent.com/57977401/100375937-92e29200-3049-11eb-8727-05b4ddf513e5.png)

<br /><br /><br />

### Nomor12
#### Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080.
- Keterangan : yyy adalah nama kelompok masing-masing. Contoh: janganlupa-ta.c01.pw
#

Jawab : <br />
**Pada UML MALANG**
1. Buka file named.conf.local dengan command ```nano /etc/bind/named.conf.local```
2. Tambahkan konfigurasi berikut dan simpan 
```
zone "janganlupa-ta.c10.pw" {
	type master;
	file "/etc/bind/janganlupa/janganlupa-ta.c10.pw";
};
```
3. Buat direktori janganlupa dengan command ```mkdir /etc/bind/janganlupa```
4. Salin file db.local ke file janganlupa-ta.c10.pw dengan command ```cp /etc/bind/db.local /etc/bind/janganlupa/janganlupa-ta.c10.pw```
5. Buka file janganlupa-ta.c10.pw dengan command ```nano /etc/bind/janganlupa/janganlupa-ta.c10.pw```
6. Tambahkan konfigurasi di bawah lalu di save
- Ubah seluruh string localhost menjadi janganlupa-ta.c10.pw.
- Tambahkan konfigurasi dibawah
```
@	    IN	NS	janganlupa-ta.c10.pw.
@       IN  A   10.151.77.91 ; IP MOJOKERTO
```
7. Lakukan ```service bind9 restart```

**TESTING**
1. Pada Mozilla Firefox, carilah pengaturan proxy dan masukkan domain yang sudah dibuat tadi (janganlupa-ta.c10.pw) dan Port 8080

![Nomor 12 Testing](https://user-images.githubusercontent.com/57977401/100376099-e228c280-3049-11eb-9e62-eb6a83d92211.png)

2. Coba masuk ke situs its.ac.id. Apabila muncul halaman seperti dibawah (dapat masuk ke situs its.ac.id), maka konfigurasi sudah benar

![Nomor 12 Testing 2](https://user-images.githubusercontent.com/57977401/100376103-e359ef80-3049-11eb-8f11-8305cddfa904.png)