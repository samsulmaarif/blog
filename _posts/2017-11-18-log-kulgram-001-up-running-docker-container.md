---
layout: post
title: Log Kulgram 001 Up & Running with Docker Container
date: '2017-11-20'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Docker
- Container
cover-img: 
  - "/img/sidareja_20160714_141402.jpg" : "Sidareja, Cilacap (2016)"
share-img: "/img/sidareja_20160714_141402.jpg"
---

# KULGRAM Docker DIMULAI

Assalamu'alaikum, selamat malam teman-teman semua. 

Perkenalkan, saya Samsul Ma'arif. Saya warga negara Indonesia asli Cilacap, Jawa Tengah yang sekarang tinggal di Turen, Kab. Malang, Jawa Timur bersama istri tercinta :-D.

Aktifitas saya sehari-hari adalah seorang A.J.I. Ada yang tau apa itu? 

Ya, betul. Antar Jemput Istri. 

Selain itu, saya juga seorang sysadmin di PuskoMedia Indonesia, sebuah perusahaan yang bergerak di bidang web hosting. 

Meskipun kata __kids zaman now__ pekerjaan sysadmin ini akan segera __punah__, tapi Alhamdulillah, hingga saat ini saya bisa beli beras, micin (eh, gak pake micin ding), bawang, brambang, dll ya dari pekerjaan ini. 

Oke, saya kira cukup perkenalan dari saya. Selanjutnya saya akan langsung masuk ke tema yang akan kita bahas pada malam hari ini adalah tentang Docker Container. 

Ada yang belum tau apa itu Container? 

## MATERI & PRAKTEK :
+ Mengenal Docker Container

|![](/img/logo-docker.jpg)|
|**Logo Docker**|

**Docker** merupakan sebuah aplikasi yang bersifat open source yang berfungsi sebagai wadah/container untuk membungkus/memasukkan sebuah perangkat lunak secara lengkap beserta semua hal yang dibutuhkan oleh perangkat lunak tersebut agar dapat berfungsi. 

Dengan Docker, seorang developer maupun sysadmin dapat membangun, mengemas, dan menjalankan aplikasi di mana pun dengan cepat dan efisien. 

Pada Docker, kita akan mengenal istilah **image**, sebuah paket ringan, berdiri sendiri, dapat dieksekusi (Ing: executable) yang mencakup semua yang dibutukan untuk menjalankan perangkat lunak, termasuk kode, runtime, pustaka, variabel lingkungan, dan berkas konfigurasi.

Lalu kita juga mengenal **Container**. Container adalah sebuah runtime dari sebuah **image**. Image akan berada di memori ketika benar-benar dieksekusi. 

Selanjutnya ada **Docker Registry**. Registry merupakan sebuah server yang menyimpan **image** di publik/private repository agar dapat diakses oleh pengguna lain. 

Docker Hub (hub.docker.com) merupakan salah satu repository publik (registry) yang dapat kita gunakan untuk menyimpan image docker yang kita buat.

Arsitektur docker menggunakan client dan server. Docker client mengirimkan request ke docker daemon untuk membangun, mendistribusikan, dan menjalankan container docker.

Keduanya docker client dan daemon dapat berjalan pada sistem yang sama. Antara docker client dan docker daemon berkomunikasi via socket (/var/run/docker.sock) menggunakan RESTful API.

|![](/img/docker-client-server.png)|
|**Arsitektur Docker**|

Docker juga memiliki port yang terdaftar di IANA, yaitu port 2375. Akan tetapi, untuk alasan keamanan port ini tidak diaktifkan secara default.

Selanjutnya, kita akan pelajari perbedaan antara VM (Virtual Machine) dengan Container.

+ VM vs Container

Jika kita pernah menjalankan VirtualBox, lalu menginstall OS di dalamnya, berarti kita membuat sebuah VM di dalam VirtualBox tersebut. 

Ada beberapa tools lain untuk membuat VM, selain VirtualBox, yaitu ada QEMU, KVM, vMWare, libvirt, dll. 

Gambar berikut mengilustrasikan perbedaan antara Docker dengan VM. 

|![](/img/docker-vs-vm.png)|
|**Tabel Perbedaan Docker dan VM**|

Dari gambar tersebut, dapat kita ketahui bahwa perbedaan antara Docker dan VM adalah bahwa Docker merupakan teknologi virtualisasi yang berada pada level sistem operasi. Berbeda dengan VM biasa yang merupakan teknologi virtualisasi pada level hardware. 

Salah satu keuntungan menggunakan Docker adalah Anda tidak memerlukan sistem operasi secara penuh setiap kali menjalankan sebuah kontainer baru.

Hal ini tentu saja mengurangi ukuran secara keseluruhan dari kontainer itu sendiri. 

Docker bergantung pada Kernel Linux OS host (yang mana hampir semua distribusi Linux menggunakan model kernel standar) 

....yang mana kontainer dibangun dengan kernel tersebut, seperti Ubuntu, CentOS, Debian, dan lain-lain.

Selain Docker, teknologi Container lain adalah :
- LXC (Linux Container); ini adalah bapak dari semua jenis Container, merepresentasikan lingkungan virtualisasi level-sistem-operasi, untuk menjalankan beberapa sistem Linux (container) dalam satu mesin Linux. Lainnya ada :
- OpenVZ
- The FreeBSD jail
- The AIX Workload partitions (WPARs)
- Solaris Containers

Pada kulgram ini tentunya saya hanya akan membahas tentang Docker. 

Oke, selanjutnya kita akan masuk ke proses instalasi. 

+ Instalasi Docker

Nah, Docker ini ada 2 varian (baca: edisi). CE (Community Edition) dan EE (Enterprise Edition). 

Docker CE ini untuk kita-kita, yang sedang menyelami teknologinya, coba-coba. Dan memang dibuat khusus untuk komunitas. 

Sedangkan Docker EE, sesuai namanya khusus untuk kalangan IT, yang membangun, mendistribusikan, dan menjalankan aplikasi bisnis pada skala produksi. 

Yang akan kita gunakan di sini Docker CE ya.... 

++ Metode Instalasi dengan manajer paket

1. Instalasi pada Ubuntu

Sebelum menginstal dari repo, Anda harus mengatur repositori Docker. Dengan demikian, Anda dapat menginstall dan memerbarui paket Docker manakala terdapat versi baru.

Install paket-paket yang diperlukan terlebih dahulu:

```bash
$ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

Tambahkan repository Docker:

```bash
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Tambahkan kunci GPG resmi Docker:

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Update lalu instal dengan perintah berikut:

```bash
$ sudo apt-get update
$ sudo apt-get install docker-ce
```

Tambahkan user saat ini ke dalam grup `docker` agar user tersebut dapat menjalankan perintah `docker` tanpa menggunakan sudo. Caranya dengan perintah berikut:

```bash
$ sudo usermod -aG docker samsul
```

Ganti `samsul` dengan nama pengguna masing-masing. Nah, agar perubahan ini dapat dirasakan, tutup terminal (atau dapat dilakukan dengan perintah `exit`), lalu buka lagi yang baru.

Untuk memudahkan teman-teman yang mengikuti kulgram ini memahami perintah yang dijalankan, saya buatkan rekamannya menggunakan `asciinema`.

Rekaman proses instalasi Docker CE di mesin saya dapat dilihat di sini : 

<script src="https://asciinema.org/a/9WmcrFmo5VXYK9cZWT2DdykLb.js" id="asciicast-9WmcrFmo5VXYK9cZWT2DdykLb" async></script>

Nah, untuk pengguna distro lain (khususnya yang sudah saya sebutkan pada pengumuman kulgram), mohon maaf saya tidak bahas satu per satu untuk instalasinya. Namun jangan khawatir, berikut link instalasi untuk distro-distro tersebut:

2. Instalasi pada openSUSE
Untuk instalasi Docker CE pada openSUSE, silakan merujuk ke sini: https://opensuse.id/2017/11/15/memasang-docker-ce-pada-opensuse-leap-42-2-dan-42-3/

3. Instalasi pada CentOS
Untuk instalasi pada CentOS, silakan merujuk ke sini : https://docs.docker.com/engine/installation/linux/docker-ce/centos/

4. Instalasi pada Debian
Untuk instalasi pada Debian, silakan merujuk ke sini : https://docs.docker.com/engine/installation/linux/docker-ce/debian/

5. Instalasi pada Fedora
Untuk instalasi pada Fedora, silakan merujuk ke sini : https://docs.docker.com/engine/installation/linux/docker-ce/fedora/

++ Metode Instalasi dengan skrip installer

Jika Anda telah menginstal Docker dengan repository, Anda tidak perlu menginstal lagi dengan cara ini. Karena pada dasarnya, skrip berikut merupakan penyederhanaan dari perintah-perintah di atas. 

Ini pun ada dua pilihan untuk menjalankannya, bisa dengan `curl`, bisa juga dengan `wget`. Pilih salah satu saja, pastikan baik `curl` maupun `wget` telah terinstall. Anda cukup menggunakan salah satunya saja.

- Dengan perintah curl:

```bash
$ sudo curl -sSL https://get.docker.io/ | sh
```

- Atau dengan perintah wget:

```bash
$ sudo wget -qO- https://get.docker.io/ | sh
```

> ~~Penggunaan skrip otomatis memaksa penggunaan AUFS sebagai sistem file yang mendasari Docker. Script ini mencoba driver AUFS, dan kemudian menginstal secara otomatis jika tidak ditemukan dalam sistem. Selain itu, juga melakukan beberapa tes dasar pada instalasi untuk memverifikasi kesehatan sistem.~~

Apakah teman-teman sudah selesai menginstall Docker? Yang sudah silakan balas dengan **sudah**.

Selanjutnya akan kita bahas mengenai perintah-perintah Docker.

+ Memahami Perintah Docker

Baiklah, kita sampai pada sub tema __Memahami Perintah Docker__. 

Docker memiliki beberapa perintah, selain perintah `docker` itu sendiri, juga perintah-perintah yang mengikutinya (selanjutnya disebut sub-perintah). Sub-sub perintah ini dibagi menjadi 2 bagian, yaitu **Managemet Command** dan **Command** saja. 

Untuk saat ini, kita hanya akan menggunakan sub-sub perintah pada bagian **Command** saja, antara lain `search`, `pull`, `images`, dan `run`. 

Sub-sub perintah yang ada selengkapnya dapat kita lihat dengan opsi **--help** di belakang perintah `docker`.

```sh
$ docker --help | less
```
Tambahan perintah `less` di belakang agar output dari perintah sebelumnya dapat discroll ke bawah. Untuk keluar dari perintah tersebut, tekan **Q** di keyboard.

Outputnya kira-kira begini : 

<script src="https://asciinema.org/a/OSgjKwRZr97ATx7L5Emvbvxkh.js" id="asciicast-OSgjKwRZr97ATx7L5Emvbvxkh" async></script>

**docker search**

Seperti terlihat pada asciinma tersebut, perintah **search** berfungsi untuk mencari image yang tersimpan di registry. 

```bash
$ docker search ubuntu
```

Hasil pencarian dari perintah tersebut hanya akan menampilkan daftar image yang tersedia di registry. Namun kita tidak dapat melihat pada masing-masing image ada tag apa saja. 

Untuk melihat daftar tag yang tersedia kita dapat memanfaatkan Docker Hub, misalnya untuk image resmi Ubuntu : https://hub.docker.com/r/_/ubuntu/ 

Di sebelah **Repo Info** terdapat **Tags**, kita akan melihat berbagai tag yang tersedia. Tag tersebut dapat berupa versi dari sebuah distro atau namakode-nya.

Daftar tag Ubuntu yang tersedia : https://hub.docker.com/r/library/ubuntu/tags/

+ Mengunduh image Docker

Setelah kita mengetahui image & tag apa yang akan kita unduh, selanjutnya kita akan mengunduh dengan sub-perintah `pull`. 

Dalam hal ini, saya akan mengunduh image **ubuntu** dengan tag **14.04**. Maka perintahnya adalah:

```bash
$ docker pull ubuntu:14.04
```
Formatnya adalah **[namaimage]:[tag]**. Jika kita mengunduh (dengan perintah `pull`) tanpa menyertakan **tag**-nya, maka tag default yang akan digunakan adalah **latest** (menjadi misalnya **ubuntu:latest**). 

<script src="https://asciinema.org/a/wD4AIfWKey3OXHDbRw1oQ1jct.js" id="asciicast-wD4AIfWKey3OXHDbRw1oQ1jct" async></script>

+ Menjalankan Container

Untuk melihat image yang beru kita unduh, kita dapat menggunakan sub-perintah `images`:

```bash
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              14.04               3aa18c7568fc        12 days ago         188MB
```

Perintah tersebut akan menampilkan image yang telah kita unduh beserta tag-nya. 

Selanjutnya untuk menjalankan image tadi, kita dapat mengunakan sub-perintah `run` sebagai berikut:

```bash
$ docker run -dti --name test1 ubuntu:14.04
7c16ad1cb35f8c5f5afd255847ab540f881daf084b57acbacbd46a8a649b218b
```
Sedikit penjelasan untuk pilihan pada sub-perintah tersebut. 
- opsi **-dti** merupakan gabungan dari **-d**, **-t**, dan **-i**
- **-d** artinya image tersebut akan dijalankan dalam mode __detach__ (baca: dijalankan di backgroud).
- **-t** mengalokasikan pseudo-TTY, ini kita perlukan agar container dapat diakses melalui sub-perintah `exec`
- **-i** interaktif
- **--name** diikuti dengan nama container, ini bebas
- setelah perintah tersebut dijalankan akan menampilkan ID berupa **hash** container yang berjalan.

Lalu kita dapat melihat container yang sedang berjalan dengan menggunakan sub-perintah `ps`.

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
cb2fe3aa1f70        ubuntu:14.04        "/bin/bash"         9 seconds ago       Up 8 seconds                            test1
```
Oke, sampe di sini kita telah berhasil mengunduh, dan menjalanan sebuah Docker container. 

Untuk lebih jelasnya dapat disaksikan dalam asciinema berikut : 

<script src="https://asciinema.org/a/r0bjJRPbWST0BRspBLI33hSO1.js" id="asciicast-r0bjJRPbWST0BRspBLI33hSO1" async></script>

**Masalah Keamanan**: Seperti yang sudah disebutkan pada tabel di atas, karena Docker berjalan di atas sebuah host, aspek keamanan Docker sendiri bergantung dari sistem operasi host yang menjalankannya. 

Ketika seseorang memiliki akses ke sistem operasi host, maka dia akan memiliki akses ke container kita. 

Karena untuk mengakses container yang sedang berjalan, dari os host kita tidak memerlukan ssh, maupun koneksi serial. Cukup menggunakan sub-perintah `exec` kita sudah dapat masuk ke dalam container tersebut.

+ Masuk ke Container yang sedang berjalan

Nah, ini yang saya maksud:

```bash
$ docker exec -ti test1 /bin/bash
root@cb2fe3aa1f70:/# uname -a
Linux cb2fe3aa1f70 4.4.0-98-generic #121-Ubuntu SMP Tue Oct 10 14:24:03 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
root@cb2fe3aa1f70:/# cat /etc/*release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.5 LTS"
NAME="Ubuntu"
VERSION="14.04.5 LTS, Trusty Tahr"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.04.5 LTS"
VERSION_ID="14.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
root@cb2fe3aa1f70:/# ls /
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@cb2fe3aa1f70:/# ls /home
root@cb2fe3aa1f70:/#
```

Oiya, sekali lagi untuk mengetahui cara penggunaan sebuah sub-perintah, tambahkan **--help** di belakangnya. `docker exec --help` akan menampilkan informasi penggunaan sub-perintah `exec`.

Lebih jelasnya mari saksikan : 

<script src="https://asciinema.org/a/rSdlMqjHK549Ml6WPELbI0c8T.js" id="asciicast-rSdlMqjHK549Ml6WPELbI0c8T" async></script>


# KULGRAM SELESAI

# Reff
- https://www.upguard.com/articles/docker-vs-lxc
- https://www.docker.com/what-container
- https://www.docker.com/what-docker
