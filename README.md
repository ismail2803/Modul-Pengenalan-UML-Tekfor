# Modul Pengenalan UML

## Apakah UML itu?
UML (User Mode Linux) adalah sebuah virtual sistem dari Linux yang memungkinkan kita untuk membuat simulasi jaringan virtual yang biasa terdiri dari host, router, dan switch.

## Instalasi UML
### 1. Untuk Windows
- **Download Putty**
Silahkan mendownload dari link berikut -> http://www.putty.org/ <br>

- **Download Xming**
Silahkan mendownload dari link berikut -> https://sourceforge.net/projects/xming/

- Jalankan Xming terlebih dahulu kemudian jalankan PuTTY.<br>
![PuTTY](/images/1.PNG) <br>

- Isikan **Host Name** dengan IP sesuai pembagian Kelompok:<br>
**Kelompok A = 10.151.36.201 <br>**
**Kelompok B = 10.151.36.202 <br>**
**Kelompok C = 10.151.36.203 <br>**
**Kelompok D = 10.151.36.205 <br>**
![PuTTY IP](/images/2.png) <br>

- Kemudian pilih tab **SSH** di bagian **Connection** dan pilih **X11**, lalu centang **Enable X11 forwarding**<br>
![PuTTY X11](/images/3.PNG) <br>

- Kemudian pilih Open. Jika muncul gambar seperti di bawah: <br>
![PuTTY Warning](/images/4.PNG) <br>
Klik **Yes** maka kemudian akan tampil window untuk login.
![PuTTY Login](/images/10.1.PNG) <br>

- Login dengan username **[nama kelompok]** dan password **praktikum**. <br>
Contoh: <br>
**Username -> a1** <br>
**Password -> praktikum** <br>
Jika berhasil maka akan muncul gambar seperti di bawah ini: <br>
![PuTTY Login Success](/images/11.1.PNG)


### 2. Untuk Linux
- Buka terminal, kemudian ketikkan `ssh -X [nama kelompok]@[ip_sesuai_pembagian]`. <br>
Contoh: `ssh -X a1@10.151.36.201`

- Saat pertama kali ssh pastikan allow connection dengan mengetikkan **yes**

- Kemudian masukkan password kelompok kalian
![PuTTY Linux](/images/ssh.png) <br>

### Membuat Topologi Jaringan yang Akan Digunakan
![Topologi](/images/topologi.png) <br>

### **PEMBAGIAN NID TUNTAP DAN NID DMZ**

**Kelompok A**
KELOMPOK | NID TUNTAP | NID DMZ
---------|------------|--------
T1 | 10.151.72.68/30 | 10.151.73.136/29
T2 | 10.151.72.72/30 | 10.151.73.144/29
T3 | 10.151.72.76/30 | 10.151.73.152/29
T4 | 10.151.72.80/30 | 10.151.73.160/29
T5 | 10.151.72.84/30 | 10.151.73.168/29
T6 | 10.151.72.88/30 | 10.151.73.176/29

**Kelompok B**
KELOMPOK | NID TUNTAP | NID DMZ
---------|------------|--------
T7 | 10.151.74.68/30 | 10.151.83.136/29
T8 | 10.151.74.72/30 | 10.151.83.144/29
T9 | 10.151.74.76/30 | 10.151.83.152/29
T10 | 10.151.74.80/30 | 10.151.83.160/29

**Kelompok C**
KELOMPOK | NID TUNTAP | NID DMZ
---------|------------|--------
T11 | 10.151.76.68/30 | 10.151.77.136/29
T12 | 10.151.76.72/30 | 10.151.77.144/29
T13 | 10.151.76.76/30 | 10.151.77.152/29
T14 | 10.151.76.80/30 | 10.151.77.160/29
T15 | 10.151.76.84/30 | 10.151.77.168/29
T16 | 10.151.76.88/30 | 10.151.77.176/29

**Kelompok D**
KELOMPOK | NID TUNTAP | NID DMZ
---------|------------|--------
T17 | 10.151.70.56/30 | 10.151.71.112/29
T18 | 10.151.70.60/30 | 10.151.71.120/29
T19 | 10.151.70.64/30 | 10.151.71.128/29
T20 | 10.151.70.68/30 | 10.151.71.136/29
T21 | 10.151.70.72/30 | 10.151.71.144/29

**Keterangan:** <br>
- **IP_eth0_SURABAYA_tiap_kelompok** = NID_tuntap_tiap_kelompok + 2
- **IP_tuntap_tiap_kelompok** = NID_tuntap_tiap_kelompok + 1
- **IP_eth1_SURABAYA_tiap_kelompok** = NID_DMZ_tiap_kelompok + 1
- **IP_MALANG_tiap_kelompok** = NID_DMZ_tiap_kelompok + 2
- **IP_MOJOKERTO_tiap_kelompok** = NID_DMZ_tiap_kelompok + 3


1. Setelah login, buat file script dengan ekstensi **.sh** yang akan digunakan untuk menyimpan script membuat **router, switch,** dan **klien**. Misalkan kita membuat file bernama **topologi.sh**.


2. Ketikkan `nano topologi.sh`


3. Sintaks yang digunakan adalah sebagai berikut: <br>
**a. Membuat switch** <br>
`uml_switch –unix NAMASWITCH > /dev/null < /dev/null &` <br>
**b. Membuat router dan klien** <br>
`xterm –T NAMADEVICE –e linux ubd0=NAMADEVICE,jarkom umid=NAMADEVICE eth0=daemon,,,NAMASWITCH mem=96M &` <br><br>

**Keterangan:**
- Sintaks untuk membuat router dan klien hampir sama, yang membedakan adalah jumlah eth-nya, eth pada router biasanya lebih dari 1.
- File **jarkom** adalah iso UML yang digunakan.
- Pembuatan jumlah router, switch, klien, dan banyaknya eth disesuaikan 
dengan topologi yang diminta.


4. Untuk topologi sesuai yang ada pada gambar, maka sintaks untuk file **topologi.sh** adalah sebagai berikut:
![Script Topologi](/images/topo_sh.png) <br>

```shell
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,'ip_tuntap_tiap_kelompok' eth1=daemon,,,switch2 eth2=daemon,,,switch1 mem=96M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=96M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=96M &

# Klien
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=96M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=96M &


```
**Keterangan:** Jangan lupa mengubah _**ip_tuntap_tiap_kelompok**_ terlebih dahulu dan sesuaikan dengan pembagian tiap kelompok masing-masing.


5. Jalankan script **topologi.sh** dengan perintah `bash topologi.sh` <br>
![UML Login](/images/surabaya1.png) <br>


6. Setelah muncul gambar seperti di atas, login pada masing-masing UML dengan menggunakan **Username = root** dan **Password = praktikum**. <br>
![UML Login Success](/images/surabaya2.png) <br>


7. Pada router **SURABAYA** lakukan setting sysctl dengan mengetikkan perintah `nano /etc/sysctl.conf`


8. Hilangkan tanda pagar (#) pada bagian `net.ipv4.ip_forward=1` <br>
![UML Sysctl](/images/surabaya3.png) <br>
Lalu ketikka `sysctl -p` untuk mengaktifkan perubahan yang ada. Dengan mengaktifkan fungsi _**IP Forward**_ ini maka Linux nantinya dapat menentukan jalur mana yang dipilih untuk mencapai jaringan tujuan.


9. Setting IP pada setiap UML dengan mengetikkan `nano /etc/network/interfaces` Lalu setting IPnya sebagai berikut:

**SURABAYA (Sebagai Router)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 'IP_eth0_SURABAYA_tiap_kelompok'
netmask 255.255.255.252
gateway 'IP_tuntap_tiap_kelompok'

auto eth1
iface eth1 inet static
address 'IP_eth1_SURABAYA_tiap_kelompok'
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.0.1
netmask 255.255.255.0
```

**MALANG (Sebagai DNS Server)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 'IP_MALANG_tiap_kelompok'
netmask 255.255.255.248
gateway 'IP_eth1_SURABAYA_tiap_kelompok'
```

**MOJOKERTO (Sebagai Web Server)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 'IP_MOJOKERTO_tiap_kelompok'
netmask 255.255.255.248
gateway 'IP_eth1_SURABAYA_tiap_kelompok'
```

**SIDOARJO (Sebagai Klien)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.0
gateway 192.168.0.1
```

**GRESIK (Sebagai Klien)**
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.3
netmask 255.255.255.0
gateway 192.168.0.1
```

**Penjelasan Pengertian:** <br>
- **IP Tuntap:** TUN yang merupakan kependekan dari Tunneling mensimulasikan layer 3, sedangkan TAP yang berarti Network Tap mensimulasikan layer 2. TUN berfungsi untuk routing, sedangkan TAP berfungsi sebagai network bridge.
- **Netmask:** Netmask adalah mask 32-bit yang digunakan untuk membagi alamat IP menjadi subnet dan menentukan host yang tersedia pada jaringan.
- **Gateway:** Jalur pada jaringan yang harus dilewati paket-paket data untuk dapat masuk ke jaringan yang lain.
- **DMZ:** DMZ adalah kependekan dari _Demilitarized Zone_, suatu area yang digunakan untuk berinteraksi dengan pihak luar. Di dalam jaringan komputer, DMZ merupakan suatu sub network yang terpisah dari sub network internal untuk keperluan keamanan.


10. Restart network dengan mengetikkan `service networking restart` atau `/etc/init.d/networking restart` di setiap UML.


11. Coba cek IP pada setiap UML dengan mengetikkan `ifconfig`. Jika sudah mendapatkan IP seperti gambar di bawah, maka setting IP yang kalian lakukan sudah benar.<br>
![UML IP](/images/surabaya4.png) <br>


12. Topologi yang dibuat sudah bisa berjalan secara lokal, tetapi kita belum bisa mengakses jaringan keluar. Ketikkan **`iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16`** pada router SURABAYA.
![UML Nat](/images/surabaya5.png) <br>
**Keterangan:**
- **iptables:** iptables merupakan suatu tools dalam sistem operasi Linux yang berfungsi sebagai filter terhadap lalu lintas data. Dengan iptables inilah kita akan mengatur semua lalu lintas dalam komputer, baik yang masuk, keluar, maupun yang sekadar melewati komputer kita. Untuk penjelasan lebih lanjut nanti akan dibahas pada Modul 5.
- **NAT (Network Address Translation):** Suatu metode penafsiran alamat jaringan yang digunakan untuk menghubungkan lebih dari satu komputer ke jaringan internet dengan menggunakan satu alamat IP.
- **Masquerade:** Digunakan untuk menyamarkan paket, misal mengganti alamat pengirim dengan alamat router.
- **-s (Source Address):** Spesifikasi pada source. Address bisa berupa nama jaringan, nama host, atau alamat IP.


13. Coba tes di semua UML dengan melakukan ping ke jaringan luar. Sebagai contoh coba `ping google.com` untuk mengecek apakah setting yang dilakukan sudah benar atau belum. <br>
![UML Ping](/images/surabaya6.png) <br>


14. Export proxy pada setiap UML dengan sintaks seperti di bawah ini: <br>
`export http_proxy=”http://[username-vpn]:[password]@proxy.its.ac.id:8080”` <br>
`export https_proxy=”http://[username-vpn]:[password]@proxy.its.ac.id:8080”` <br>
`export ftp_proxy=”http://[username-vpn]:[password]@proxy.its.ac.id:8080”` <br>


15. Setelah itu, lakukan update pada setiap UML dengan mengetikkan `apt-get update`


16. Terakhir, untuk mematikan UML **JANGAN langsung diclose**. Ketikkan `halt` pada setiap UML untuk mematikannya. Alternatif lain adalah dengan membuat script dengan ekstensi **.sh** supaya mempermudah serta mempercepat kalian untuk mematikannya. Sebagai contoh buat file bernama **bye.sh** dan tuliskan sintaks seperti di bawah ini:
![UML Bye](/images/halt.png) <br>
```
uml_mconsole SURABAYA halt
uml_mconsole MALANG halt
uml_mconsole MOJOKERTO halt
uml_mconsole SIDOARJO halt
uml_mconsole GRESIK halt

```
Simpan script yang sudah dibuat kemudian jalankan dengan mengetikkan `bash bye.sh`

  
  
