---
layout: post
title: Catatan Membuat Pabrik CD BlankOn di Debian 9
date: '2017-10-20'
author: Samsul Maarif
categories: blog
tags:
- Linux
- BlankOn
- Infrastrktur
bigimg: 
  - "/img/pantai-balekambang_20160724_094830.jpg" : "Pantai Balekambang, Malang, Jawa Timur (2016)"
share-img: "/img/pantai-balekambang_20160724_094830.jpg"
---

Pabrik CD merupakan sebuah alat untuk membuat berkas ISO dari sebuah distro Linux. BlankOn Linux merupakan distro yang dikembangkan oleh YPLI dan komunitas pengembang BlankOn. Berdasarkan [*commit history*, Pabrik CD](https://github.com/BlankOn/pabrik-cc/commits/master?after=31d818193de84a3bd2bd44cfc447679dc1569d5a+69) pertama kali dikembangkan pada 13 November 2011 oleh pak Mohammad Anwari (MDAMT). Saat itu beliau adalah Koordinator pengembang BlankOn untuk rilis pertama versi 1.0. 

Pabrik CD kini menjadi alat yang cukup matang untuk dapat digunakan oleh pengembang BlankOn. Nah, berikut kurang-lebih langkah-langkah menggunakannya. 

Pasang perkakas yang akan digunakan. Dalam praktek ini, saya menggunakan Debian 9 (Stretch). Ada sedikit tambahan paket yang perlu diinstall, yaitu `time`, `binutils`, dan `bc`. Berikut lengkapnya :

```sh
$ sudo apt-get install git debootstrap genisoimage zsync reprepro \
	xorriso postfix mailutils dosfstools time binutils bc
```

## Setup GNUPG

GNUPG merupakan suatu software enkripsi yang mengimplementasikan RFC2440. Penggunaan program ini biasanya ditemui pada enkripsi email atau sebagai digital signature. Model enkripsi yang digunakan adalah PKI(Public Key Infrastructure). Mari baca dan praktekkan, klik [di sini](https://help.github.com/articles/generating-a-new-gpg-key/).

## Membuat Pabrik CD

* Buat akun pengguna CDIMAGE, akun ini akan digunakan selama proses pembuatan CD.

```sh
$ sudo adduser cdimage
```

* Ubah CDIMAGE menjadi sudoers, untuk beberapa tugas (di belakang layar) akun CDIMAGE memerlukan akses **sudo**

```sh
$ sudo visudo -f /etc/sudoers
```

Tambahkan di baris terakhir dan simpan

```sh
cdimage ALL=NOPASSWD: ALL
```

Dengan demikian, akun CDIMAGE akan dapat menjalankan perintah dengan hak akses root melalui `sudo` tanpa dimintai password.

* Masuk Ke CDIMAGE  

```sh
$ sudo su – cdimage
```

* Mengunduh Skrip Pabrik CD dari repository BlankOn. 

```sh
$ git clone https://github.com/blankon/pabrik-cc.git  
$ git checkout uluwatu
```

* Ubah suai config pabrik  

```sh
$ vim uluwatu.config
```

* Mengatur debootstrap uluwatu  

```sh
$ vim uluwatu.debootstrap
$ sudo cp uluwatu.debootstrap /usr/share/debootstrap/scripts/uluwatu
$ sudo cp lsb-release /etc/
```

* Mengatur lokasi cd image  

```sh
$ mkdir -p /home/cdimage/images/livedvd-harian/
```

## Membuat Cetakan CD

Untuk memulai menjalankan ini, pastikan koneksi dan kuota internet cukup. Karena ini akan memakan kurang lebih 1,5 - 2,5 GB untuk sekali jalan. Belum lagi jika ada galat dan harus mengulangi.

```sh
$ ./enter-cd-blankon.sh
```

## Catatan Sidik Gangguan

Nah, ini agak sedikit rumit, iya, hanya sedikit saja. Karena pesan kesalahan (baca: *error*), tidak langsung nampak di layar. Perlu dicari lognya, karena setiap kali build, tempat lognya berbeda (dinamis). Tergantung nomor build image yang sedang dibuat. Secara umum, lokasinya adalah di `pabrik-cc/tmp/NOMORBUILD-publish/log.txt`. Kalau di dalam skrip, NOMORBUILD di sini adalah `$DISK_ID`.

Untuk melihat log ini, dapat dilakukan dengen membuka terminal baru, lalu ketikkan perintah:

```sh
$ tailf /home/cdimage/pabrik-cc/tmp/NOMORBUILD-publish/log.txt
```

Misalnya dapat log galat seperti ini:

```
Build log:
./build-image: line 78: /usr/bin/time: No such file or directory
```
Solusinya : 

```sh
$ sudo apt install time
```

Coba jalankan lagi `./enter-cd-blankon.sh`, lalu menemukan galat lagi:

```
/usr/sbin/debootstrap: 60: /usr/sbin/debootstrap: ar: not found
```

Googling sebentar, akhirnya dapat solusinya:

```sh
$ sudo apt install binutils
```

Ternyata belum selesai sampai di situ, masih menemukan galat:

```
Generating disk info                                                     
./pabrik-cc/init-variables: line 35: bc: command not found
```

Tapi syukurlah, masalah tersebut dapat terselesaikan dengan perintah berikut:

```sh
$ sudo apt install bc
```

Setidaknya, ini yang saya alami. Kalau Anda ingin mencoba juga, semoga tidak muncul galat seperti saya. Asalkan perkakas yang diinstall sudah lengkap. Lain hal kalau yang galat dari sisi repositorinya. Kita tunggu saja reponya normal. Atau, jadilah pengembang BlankOn untuk ikut memperbaiki masalah yang ada :-D.

## Pointing Domain

Pointing domain ini diperlukan, kalau Anda ingin orang lain dapat mengunduh hasil dari pabrik CD yang Anda buat melalui alamat yang mudah diingat. Syaratnya harus punya IP Publik dan domain yang sudah terdaftar. Kalau Anda memesan VPS, mestinya Anda punya IP Publik. 

Bagaimana cara pointing domain? Silakan [baca di sini](http://wiki.samsul.web.id/linux/DNS.Server.Ubuntu), kurang lebih sama lah, antara Ubuntu dan Debian. 

> Kalau berupa sub domain, seperti yang saya gunakan di sini `cdimage.samsul.web.id` arahkan A record ke IP server. 
> <br>`cdimage	IN	A	103.103.1.17`

## Nginx web server

Nah, selain domain, supaya citra CD/DVD berupa berkas \*.iso dapat diunduh, tentu saja membutuhkan web server. Dalam hal ini saya akan (baca: telah) menggunakan `nginx`. Install terlebih dulu web servernya:

```sh
$ sudo apt install nginx
```

Lalu konfigurasikan virtualhostnya. Salin berkas konfigurasi default

```sh
$ cd /etc/nginx/sites-available/
$ sudo cp default cdimage
```

Sunting berkas konfigurasi cdimage

```sh
$ sudo vim cdimage
```

Tambahkan/sesuaikan konfigurasinya sebagai berikut :

```
server {                                  
        listen 80;

	root /home/cdimage/images/livedvd-harian/;

	index index.html index.htm index.nginx-debian.html;

	server_name cdimage.samsul.web.id;

        location / {
                autoindex on;
                try_files $uri $uri/ =404;
        }
}
```

> CATATAN: pastikan modul autoindex aktif (on). 

Simpan berkas konfigurasinya dengan mengetikkan **:wq**, lalu Enter. Buat simbolik link berkas konfigurasi `cdimage`:

```sh
$ cd ../sites-enabled/
$ sudo ln -s ../sites-available/cdimage
```

Selanjutnya restart nginx dan bind9 :

```sh
$ sudo systemctl restart bind9
$ sudo systemctl restart nginx
```

Cek hasilnya melalui peramban web.

|![](/img/cdimage.samsul.web.id.png)|
|Tampilan **cdimage.samsul.web.id**|

Demikian catatan saya kali ini. Semoga bermanfaat. Selamat mencoba.
