---
layout: post
title: Berkenalan dengan Docker
date: '2017-06-06'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Docker
- Container
bigimg: 
  - "/img/sidareja_20160714_141402.jpg" : "Sidareja, Cilacap (2016)"
share-img: "/img/sidareja_20160714_141402.jpg"
---

Sejak beberapa bulan/minggu ini saya ingin sekali menulis tentang Docker. Baru kali ini kesempatan menulis, di sela-sela mengikuti pelatihan kurikulum dua ribu tiga belas. Ya, saya seorang pengajar/pendidik/guru di sebuah sekolah swasta di lingkungan Kecamatan Kawunganten, Kabupaten Cilacap. Baiklah, tidak perlu panjang lebar, mari mulai membahas tentang Docker.

Docker awal mulanya dikembangkan oleh Solomon Hykes pada tahun 2009, sebagai project internal di dotCloud, sebuah perusahaan Paas (platform as a service). Namun, pada tahun 2013 dotCloud diubah menggunakan nama Docker hingga sekarang. 

> **Docker** merupakan sebuah aplikasi yang bersifat open source yang berfungsi sebagai wadah/container untuk membungkus/memasukkan sebuah perangkat lunak secara lengkap beserta semua hal yang dibutuhkan oleh perangkat lunak tersebut agar dapat berfungsi. Dengan Docker, seorang developer maupun sysadmin dapat membangun, mengemas, dan menjalankan aplikasi di mana pun dengan cepat dan efisien. 

Dengan Docker Anda dapat dengan mudah membuat `image` dan lingkungan virtual pada laptop dan menjalankan perintah atau operasi di dalamnya. Aksi yang dijalankan pada kontainer yang berjalan pada mesin lokal Anda akan sama dengan lingkungan saat Docker berjalan pada mesin **produksi**.

Arsitektur docker menggunakan client dan server. Docker client mengirimkan request ke docker daemon untuk membangun, mendistribusikan, dan menjalankan container docker. Keduanya docker client dan daemon dapat berjalan pada sistem yang sama. Antara docker client dan docker daemon berkomunikasi via socker menggunakan RESTful API.

|![](/img/docker-client-server.png)|
|**Arsitektur Docker**|

## Perbedaan Docker dengan VM

Gambar berikut mengilustrasikan perbedaan antara Docker dengan VM. 

|![](/img/docker-vs-vm.png)|
|Perbedaan Docker dan VM|

Dari gambar tersebut, dapat kita ketahui bahwa perbedaan antara Docker dan VM adalah bahwa Docker merupakan teknologi virtualisasi yang berada pada level sistem operasi. Berbeda dengan VM biasa yang merupakan teknologi virtualisasi pada level hardware. 

Salah satu keuntungan menggunakan Docker adalah Anda tidak memerlukan sistem operasi secara penuh setiap kali menjalankan sebuah kontainer baru. Hal ini tentu saja mengurangi ukuran secara keseluruhan dari kontainer itu sendiri. Docker bergantung pada Kernel Linux OS host (yang mana hampir semua distribusi Linux menggunakan model kernel standar) yang mana kontainer dibangun dengan kernel tersebut, seperti Ubuntu, CentOS, Debian, dan lain-lain.

| **Mesin Virtual (VM)** | **Kontainer** |
|Representasi virtualisasi level perangkat keras|Representasi virtualisasi level sistem operasi|
|Berat|Ringan|
|Penyediaan lambat|Penyediaan waktu-nyata dan skalabilitas|
|Performa terbatas|Performa asli|
|Terisolasi penuh dan karenanya lebih aman|Isolasi level proses dan karenanya kurang aman|

> Tabel ini merupakan terjemahan bebas dari buku elektronik **Learning Docker** ditulis oleh Pethuru Raj, dkk. yang diterbitkan oleh PACT Publishing.

## Menginstall Docker di Ubuntu

Ada beberapa metode untuk menginstall Docker. Pertama Anda dapat menginstall dengan paket yang tersedia di repository. Cara kedua adalah dengan menggunakan script installer yang telah disediakan oleh komunitas Docker. 

Cara lainnya adalah dengan mengunduh paket **.deb**, lalu menginstallnya dengan perintah `dpkg`. Cara ini tentu saja berguna ketika Anda ingin menginstall namun koneksi internet terbatas. Namun cara ini tidak akan saya tuliskan di sini. Toh, ketika Anda membaca tulisan ini, Anda sudah terhubung ke internet, kan?

### Menginstal dari Repository

Sebelum menginstal dari repo, Anda harus mengatur repositori Docker. Dengan demikian, Anda dapat menginstall dan memerbarui paket Docker manakala terdapat versi baru.

Install paket-paket yang diperlukan terlebih dahulu:

```bash
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

Tambahkan kunci GPG resmi Docker:

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo apt-key add -
```

Update lalu instal dengan perintah berikut:

```bash
$ sudo apt-get update
$ sudo apt-get install docker-ce
```

### Menginstall menggunakan Skrip Installer

Jika Anda telah menginstal Docker dengan repository, Anda tidak perlu menginstal lagi dengan cara ini. Karena pada dasarnya, skrip berikut merupakan penyederhanaan dari perintah-perintah di atas. Ini pun ada dua pilihan untuk menjalankannya, bisa dengan `curl`, bisa juga dengan `wget`. Pilih salah satu saja, pastikan baik `curl` maupun `wget` telah terinstall.

- Dengan perintah curl:

```bash
$ sudo curl -sSL https://get.docker.io/ | sh
```

- Dengan perintah wget:

```bash
$ sudo wget -qO- https://get.docker.io/ | sh
```

> Penggunaan skrip otomatis memaksa penggunaan AUFS sebagai sistem file yang mendasari Docker. Script ini mencoba driver AUFS, dan kemudian menginstal secara otomatis jika tidak ditemukan dalam sistem. Selain itu, juga melakukan beberapa tes dasar pada instalasi untuk memverifikasi kesehatan sistem.

## Memahami pengaturan Docker

Beberapa komponen Docker dan versinya, storage, eksekusi drivernya, lokasi berkasnya, an sebagainya penting untuk dipahami. Dengan memahami pengaturan Docker Anda akan menemukan apakah instalasi Docker berhasil atau tidak. Anda dapat menggunakan dua sub-perintah docker untuk mengetahuinya, antara lain `docker version` dan `docker info`.

```bash
$ sudo docker version
[sudo] password for samsul: 
Client:
 Version:      1.12.3
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   6b644ec
 Built:        Mon, 19 Dec 2016 09:20:48 +1300
 OS/Arch:      linux/amd64

Server:
 Version:      1.12.3
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   6b644ec
 Built:        Mon, 19 Dec 2016 09:20:48 +1300
 OS/Arch:      linux/amd64
```

Nah, perintah tersebut menampilkan banyak baris teks, sebagai pengguna Docker, Anda perlu mengetahui beberapa komponen berikut:

- Versi klien
- Versi API klien
- Versi server
- versi API server

Baik server maupun klien, versi di mesin saya saat ini adalah **1.12.3** dan versi API klien dan servernya adalah **1.24**.

Jika kita membedah internal dari sub-perintah `docker version`, maka pertama akan menampilkan daftar informasi terkait klien yang disimpan secara lokal. Lalu, hal tersebut akan membuat panggilan REST API ke server melalui HTTP untuk mendapatkan rincian terkait server.

Mari selami lebih jauh tentang lingkungan Docker dengan sub-perintah `docker info`:

```
$ sudo docker -D info
Containers: 9
 Running: 0
 Paused: 0
 Stopped: 9
Images: 12
Server Version: 1.12.3
Storage Driver: aufs
 Root Dir: /var/lib/docker/aufs
 Backing Filesystem: extfs
 Dirs: 101
 Dirperm1 Supported: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: overlay bridge host null
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Security Options: apparmor seccomp
Kernel Version: 4.4.0-78-lowlatency
Operating System: Ubuntu 16.04.2 LTS
OSType: linux
Architecture: x86_64
CPUs: 2
Total Memory: 3.541 GiB
Name: studio
ID: IOTJ:QBGQ:UOCC:DGSI:6IDM:4XIS:7YYQ:KGY5:4WEO:VERA:LL3V:3IYM
Docker Root Dir: /var/lib/docker
Debug Mode (client): true
Debug Mode (server): false
Username: samsulmaarif
Registry: https://index.docker.io/v1/
WARNING: No swap limit support
Insecure Registries:
 127.0.0.0/8

```

Seperti terlihat di situ, Docker di laptop saya sudah saya install beberapa bulan yang lalu (gak nyampe tiga bulan sih kalo gak salah:-D ), sudah ada 9 kontainer dan 12 image yang ada di laptop ini. Driver storage menggunakan **aufs**, dan lokasi Root direktorinya ada di `/var/lib/docker/aufs`. Perintah ini juga menampilkan detail informasi seperti Versi Kernel, Sistem operasi, jumlah CPU, total Memory, Nama, dan nama host Docker.

## Mengunduh image Docker pertama Anda

Setelah berhasil menginstall Docker, langkah berikutnya adalah mengunduh image dari Docker registry. Docker registry adalah sebuah repositori aplikasi yang menyimpan berbagai aplikasi dari image Linux dasar hingga aplikasi yang canggih. Perintah `docker pull` digunakan untuk mengunduh image dari registry. Pada bagian ini, Anda dapat mengunduh versi kecil dari Linux bernama **busybox**, coba saja:

```
$ sudo docker pull busybox
[sudo] password for samsul: 
Using default tag: latest
latest: Pulling from library/busybox
1cae461a1479: Pull complete 
Digest: sha256:c79345819a6882c31b41bc771d9a94fc52872fa651b36771fbe0c8461d7ee558
Status: Downloaded newer image for busybox:latest
```

Setelah image diunduh, Anda dapat melihatnya dengan sub-perintah `docker images` :

```
$ sudo docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
samsulmaarif/opensid-aio   latest              064628a21c22        2 weeks ago         701.3 MB
samsulmaarif/opensid       latest              afa37abb5e92        2 weeks ago         390.5 MB
busybox                    latest              c75bebcdd211        2 weeks ago         1.106 MB
nginx                      latest              46102226f2fd        5 weeks ago         109.4 MB
ubuntu                     latest              6a2f32de169d        7 weeks ago         117.2 MB
samsulmaarif/ubuntu        latest              968b9eb0eb6c        7 weeks ago         129.8 MB
```

Kok jadinya banyak? kan baru unduh **busybox**. Iya, karena saya sudah mengunduhnya lebih dulu. Jika mau, Anda dapat pula mengunduh image yang saya buat misalnya `samsulmaarif/ubuntu` ya tinggal diunduh dengan 

```
$ sudo docker pull samsulmaarif/ubuntu
```

Etapi, ini akan memakan kuota internet yang lumayan loh yah. Kalau Anda sedang irit, fakir benwit seperti saya baiknya nyari wifi gratisan :-P

## Menjalankan image yang telah diunduh

Mari abaikan **busybox**, di sini kita akan menjalankan image **ubuntu** atau **samsulmaarif/ubuntu** saja. Karena menjalankan docker itu cukup mudah, gunakan sub-perintah `docker run` sebagai berikut:

```
$ sudo docker run -t -i --name busi busybox
[sudo] password for samsul: 
/ # pwd
/
/ # ls
bin   dev   etc   home  proc  root  sys   tmp   usr   var
```

Atau jika Anda menjalankan yang lain:

```
$ sudo docker run -t -i --name mendoan samsulmaarif/ubuntu /bin/bash
root@16bb9efc70b3:/# cat /etc/*release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.1 LTS"
NAME="Ubuntu"
VERSION="16.04.1 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.1 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
```

Keren, kan? Untuk keluar dari lingkungan Docker, sama halnya ketika Anda log-out melalui terminal: cukup ketikkan `exit`.

```
root@16bb9efc70b3:/# exit
exit
$
```

Kiranya cukup sekian catatan kali ini. Semoga lain waktu dapat disambung lagi. Baik di topik yang sama maupun berbeda. Yah, meskipun di awal catatan ini ditulis saat istirahat pelatihan kurtilas. Nyatanya tulisan ini saya selesaikan saat jam makan sahur. Selamat berpuasa!

## Referensi

- [Panduan resmi instalasi Docker](https://docs.docker.com/engine/installation/linux/ubuntu/)
- Sebagian besar materi di sini merupakan terjemahan bebas dari ebook **Learning Docker** dan **Mastering Docker** yang diterbitkan oleh [Pact Publishing](https://www.packtpub.com/packt/offers/free-learning). Ebook tersebut didapatkan secara gratis dari penerbit yang diumumkan melalui [kanal Telegram](https://t.me/packtpubfreelearning).
- <https://en.wikipedia.org/wiki/Docker_(software)>
- <https://www.dewaweb.com/tutorial-docker-dalam-bahasa-indonesia/>
