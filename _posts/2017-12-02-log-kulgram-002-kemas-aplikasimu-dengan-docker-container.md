---
layout: post
title: Log Kulgram 002 Kemas Aplikasimu dengan Docker Container
date: '2017-12-02'
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

### KULGRAM #002 Docker Container DIMULAI ###

|![](/img/kulgram-docker2.jpg)|
|**banner kulgram #002**|

Assalamualaikum, selamat malam temen-temen semua... 

Bagaimana kabarnya malam Minggu ini? 

Nah, kira-kira saya perlu perkenalan lagi gak nih? hehehe....

Perkenalkan (lagi), saya Samsul Ma'arif. Saya warga negara Indonesia asli Cilacap, Jawa Tengah yang sekarang tinggal di Turen, Kab. Malang, Jawa Timur bersama istri tercinta :-D.

Aktifitas saya sehari-hari adalah seorang A.J.I. Ada yang tau apa itu? 

Ya, betul. Antar Jemput Istri.  hehehehe...

Selain itu, saya juga seorang sysadmin di PuskoMedia Indonesia, sebuah perusahaan yang bergerak di bidang web hosting.

Sebelumnya saya ucapkan terima kasih untuk pak Septi (@kangsepti) founder grup ini telah memberikan kesempatan untuk saya sharing tentang Docker Container. 

Baik temen-temen semua, seperti yang telah saya tuliskan di pengumuman kulgram kali ini. 

Bahwa prasyarat mengikuti kulgram kali ini adalah telah membaca log kulgram sebelumnya. 

Ini supaya apa yang saya sampaikan pada kulgram #001 tidak perlu saya ulangi lagi. :v Lagipula di grup ini pun masih dapat di baca kulgram sebelumnya... hehehe

Oiya, mungkin perlu saya sampaikan bahwa materi ini masih merupakan materi dasar untuk memahami docker. 

Jadi belum sampai tahap implementasi. Karena terus terang saya sendiri baru belajar tentang docker ini. 

Karena kata guru saya, cara belajar yang paling efektif adalah dengan cara __mengajar__. Kita sama-sama belajar di sini. 

Kalo ada yang salah, mohon dikoreksi. 

### MATERI & PRAKTEK ###

|![](/img/logo-docker.jpg)|
|**Logo Docker**|

**+ Lebih lanjut Perintah Docker**

Kalau pada kulgram sebelumnya kita sudah belajar menggunakan sub-sub perintah `pull`, `run`, `images`, `exec`, dan `search`.

Kali ini kita akan belajar menggunakan beberapa perintah lain, `rm`, `rmi`, `ps`, `port`, `inspect`, `commit`, `login`, `logout`.

Namun sebelumnya, kita akan menggunakan perintah untuk menjalankan docker image yang sudah diunduh pada kulgram sebelumnya. 

```
docker run -dti --name app1 ubuntu:14.04
```

Oke, setelah kita menjalankan image ubuntu. kita coba install beberapa aplikasi, atau modifikasi beberapa berkas. 

Sekarang masuk ke container docker yang sedang running tersebut. 

```
docker exec -ti app1 /bin/bash
```

Coba lakukan update, lalu install python.

tidak harus python ya.... bebas... yang penting pada praktek ini temen-temen paham.... bahwa proses ini kita sedang memodifikasi container yang sedang dijalankan...

```
root@bb565d4e6aae:/# apt update && apt install python
```

setelah berhasil, keluar dari container dengan perintah `exit`.

seperti kulgram sebelumnya, untuk memudahkan teman-teman memahami prosesnya, saya buatkan rekamannya dengan `asciinema` : 

<script src="https://asciinema.org/a/WDtER6vkWcdHQr7DkIJ1L7Wlr.js" id="asciicast-WDtER6vkWcdHQr7DkIJ1L7Wlr" async></script>

Setelah menginstall aplikasi yang dipilih atau memodifikasi konten dalam kontainer tersebut, kita dapat menyimpannya dengan sub-perintah `commit` sebagai berikut :


```
docker commit -a "Samsul Maarif" -m "Ini appku" app1 samsulmaarif/app1
```

Perintah tersebut digunakan untuk menyimpan container yang telah dimodifikasi dengan nama baru. Nama di sini terdiri dari nama repository (dalam hal ini saya menggunakan nama akun **samsulmaarif** sebagai repository lalu **app1** sebagai nama image-nya).

Teman-teman juga dapat menggunakan format **[repository]:[tag]**. Tag biasanya berupa nomor versi aplikasinya. Kalau tanpa menggunakan tag, secara otomatis akan tersemat tag **latest** di belakangnya. 

Nah, beberapa opsi yang saya gunakan adalah:

- `-a`; untuk menentukan __author__ (baca: pembuat image)
- `-m`; ini mirip dengan commit message di git.
- diikuti dengan **nama container** dan **repository**.

**nama container** di sini merupakan nama saat kita menjalankan `docker run` dengan opsi `--name namacontainer`. 

Dalam hal ini nama container yang saya gunakan adalah `app1`

Jika teman-teman pernah menggunakan VCS (Version Control System) seperti Git, Subversion, Bazaar, dll, teman-teman akan cepat memahami konsep docker. 

Untuk melihat container baru yang telah kita buat, jalankan perintah `docker images`.

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
samsulmaarif/app1   latest              5d16c781e77a        6 seconds ago       233MB
ubuntu              14.04               3aa18c7568fc        3 weeks ago         188MB
```

kalau kurang jelas dapat pula diintip di sini : http://tempel.blankon.in/5951237

Dari hasilnya dapat kita lihat, image asli berukuran 188MB sedangkan image yang telah kita modifikasi 233MB. 

tentunya tergantung dari banyaknya aplikasi yang diinstall di sana....

<script src="https://asciinema.org/a/MP4VOmJLJgU0mXe19ovJMhDSx.js" id="asciicast-MP4VOmJLJgU0mXe19ovJMhDSx" async></script>

Nah, apakah teman-teman sudah memiliki akun Docker ID (hub.docker.com) ? Yang sudah silakan balas chat ini dengan **sudah**.

baiklah, untuk teman-teman yang sudah memiliki akun, bisa mengikuti langkah selanjutnya. bagi yang belum silakan membuat akun, atau menyimak materi selanjutnya.

yang dipersiapkan: nama akun & password jangan lupa!! 

Sekarang kita akan menggunakan sub-perintah `login` sebagai berikut :

```
docker login
```

Teman-teman akan diminta untuk memasukkan **Username** dan **password** Docker ID. 

**CATATAN:** 
Password tidak akan nampak, jadi ketikkan saja passwordnya, lalu tekan Enter. 

Jika teman-teman berhasil login, akan muncul pesan **Login Succeeded**. Kalau sudah seperti ini kita akan melangkan ke perintah selanjutnya.

Pastikan nama repository yang akan kita gunakan dengan mengetikkan ulang sub-perintah `images`

```
docker push samsulmaarif/app1
```

Ganti **samsulmaarif/app1** dengan nama repository & nama aplikasi teman-teman sendiri. 

Nah, teman-teman akan merasakan apa manfaatnya internet cepat. 

Jadi kalo pak menteri tanya, **internet cepat buat apa?** teman-teman sudah punya jawabannya ya.... hehehehe....

Nah, ini rekaman push image saya : 

<script src="https://asciinema.org/a/hOCMHj90ZSmigeBZKUELJqvyz.js" id="asciicast-hOCMHj90ZSmigeBZKUELJqvyz" async></script>

Hasil dari image yang telah diunggah selanjutnya dapat diunduh oleh __orang lain__ (kita sendiri, kalau itu private) dengan memanfaatkan sub-perintah `pull`. 

Misal untuk image yang saya buat tadi, dapat diunduh dengan perintah :

```
docker pull samsulmaarif/app1
```

Image yang saya buat tersebut dapat dilihat di : https://hub.docker.com/r/samsulmaarif/app1/

## Menjalankan Aplikasi Web dengan Docker ##

Oke, setelah kita dapat membuat image dengan cara modifikasi image lalu menyimpannya dengan sub-perintah `commit`. 

Juga kita telah mencoba untuk mengunggah image tersebut ke Docker Hub.

Kali ini kita akan mencoba sesuatu yang (tidak cukup) baru (sebenarnya), tapi menarik untuk kita coba. 

Nah, pada dasarnya bukan hanya image sistem operasi saja yang dapat kita unduh dari Docker Hub. 

Namun image-image aplikasi yang siap pakai pun tersedia. Teman-teman dapat mengeksplorasi dengan memanfaatkan pencarian di hub.docker.com atau sub-perintah `search` di teriminal. 

Pada praktek ini saya akan menggunakan image `nginx` yang resmi (Official build). http://tempel.blankon.in/5951239.txt 

```
docker pull nginx
```

Setelah diunduh, jalankan dengan perintah :

```
docker run -dti --name web1 -p 8000:80 nginx:latest
```

Lalu coba akses menggunakan browser favirit Anda ke http://localhost:8000 

Ganti **localhost** dengan alamat IP komputer di mana temen-temen menjalankan container nginx tadi. 

unduh & run image nginx : 

<script src="https://asciinema.org/a/8tKygdZJ9MTSLzPSBpUponrlx.js" id="asciicast-8tKygdZJ9MTSLzPSBpUponrlx" async></script>

Teman-teman juga dapat mencoba aplikasi lain, misalnya wordpress. 

Nah, untuk mengunduh dan menjalankan aplikasi wordpress ini teman-teman dapat membaca di https://hub.docker.com/_/wordpress/


wordpress : http://tempel.blankon.in/5951240.txt

```
docker pull wordpress
```

Unduh juga image mysql, dalam hal ini saya akan menggunakan tag (versi) 5.5

```
docker pull mysql:5.5
```

Kenapa kita mengunduh image mysql juga?

karena wordpress membutuhkan sebuah database server....


Jalankan container mysql terlebih dahulu sebelum menjalankan yang wordpress....

Gunakan perintah berikut untuk menjalankannya... 

```
docker run -d -p 3306:3306 --name DB -e MYSQL_ROOT_PASSWORD=rahasia mysql:5.5
```

## Membuat Image Docker dengan Dockerfile ##

**Dockerfile** merupakan sebuah berkas yang dapat digunakan untuk membangun sebuah image docker. 

Dengan **Dockerfile**, kita dapat mengemas sebuah aplikasi lengkap dengan segala kebutuhannya. Tentunya dengan mendefinisikannya di dalam berkas tersebut. 

Mari kita mulai dengan sebuah contoh. Oiya, agar tidak tercampur dengan berkas lain kita akan membuat sebuah direktori terlebih dulu. 

Dalam hal ini folder yang saya maksud akan saya beri nama **Nacita**. Teman-teman bebas menggunakan nama apa pun.

Buat folder/direktorinya

```
mkdir Nacita
cd Nacita
```

Lalu buat berkas kosong **Dockerfile**

```
touch Dockerfile
```

Edit berkas tersebut, lalu isikan dengan : http://tempel.blankon.in/5951241.txt

Baik, akan saya bahas baris per baris konten dari berkas tersebut. 

Format instruksinya adalah sebagai berikut: 

```
# Komentar, semua yang diawali dengan tanda pagar (#) adalah komentar dan akan diabaikan oleh sistem

INSTRUKSI argumen
```

di mana instruksi seperti tertulis pada **Dockerfile** yang tadi saya buat terdiri dari **FROM**, **MAINTAINER**, **RUN**, **ADD**, **EXPOSE**, dan **CMD**. 

instruksi lain akan dibahas jika waktunya cukup.

Yang pertama, **FROM**; baris ini menunjukkan image dasar yang akan digunakan.

**MAINTAINER**; seperti nama instruksinya berisi nama maintainer dari image yang sedang dibangun. Tentunya ini nama kita sendiri.

**RUN**; baris ini menunjukkan perintah yang dijalankan di dalam container saat proses build. satu berkas **Dockerfile** dapat berisi beberapa baris instruksi RUN, sesuai kebutuhan.

**ADD**; instruksi ini memiliki 2 argumen. argumen pertama adalah nama berkas/direktori di lokal diikuti dengan nama/lokasi berkas di dalam container. 

**EXPOSE**; ini adalah port yang dibuka dalam sebuah container. Karena dalam contoh ini adalah web server, port default web adalah 80.

Lalu yang terakhir adalah **CMD**; instruksi ini umumnya berada di baris paling bawah. Menginstruksikan perintah yang dieksekusi saat sebuah container dijalankan.

baik, untuk keperluan ini saya mengubah berkas Dockerfile saya menjadi http://tempel.blankon.in/5951244

dengan penambahan baris 9, `ADD app /app`
lalu buat file baru dengan nama **000-default.conf**

isi berkas tersebut dengan ini : http://tempel.blankon.in/5951245

seperti terlihat di sana, saya mendefinisikan **DocumentRoot** ke **/app**

lalu saya buat sebuah direktori baru dengan nama app

```
mkdir app
cd app
```

lalu saya buat sebuah berkas baru bernama index.html lalu isi dengan : http://tempel.blankon.in/5951246

nah sekarang tampak seperti ini :

```
samsul@kulgram-docker:~/Nacita/app$ cd ..
samsul@kulgram-docker:~/Nacita$ ls
000-default.conf  app  Dockerfile
```

sampai di sini, kita telah menyiapkan sebuah aplikasi berupa file index.html, sebuah berkas konfigurasi 000-default.conf dan sebuah berkas Dockerfile

nah, ini saat yang menegangkan....

eh, gak tegang sih... cuma akan memakan kuota internet lagi... hahahah...

oke, temen-temen siap?

silakan jalankan perintah ini....

```
docker build -t aplikasiku .
```

argumen setelah -t terserah temen-temen mau kasih nama apa...

tunggu sampe proses ini selesai...

sampai di sini ada pertanyaan lagi?

bisa dipahami penjelasan-penjelasan saya?

iyes, karena repository defaultnya mengambil main server.... hahahaha

nanti saya upload asciinema-nya juga...

nah, ini hasilnya... : 

<script src="https://asciinema.org/a/IVgh7RrJh5ie9VvHZl8VRtmd2.js" id="asciicast-IVgh7RrJh5ie9VvHZl8VRtmd2" async></script>

proses ini saya tidak ada kendala build... jika temen-temen mendalami gagal build, baca lognya... pada saat build kan ketahuan salahnya di mana....

biasanya kesalahan ketik di **Dockerfile**, atau hal lain... koneksi internet misalnya...

nah, ibaratnya begini, kita mau membuat sebuah image docker untuk sebuah aplikasi.

kita pelajari dulu, aplikasi itu butuh apa saja...

lalu terjemahkan kebutuhan tersebut ke dalam berkas Dockerfile, lalu coba build...

kalau terjadi kesalahan, baca lognya, perbaiki kesalahannya, lalu coba build lagi... begitu seterusnya... heeehe....

satu baris perintah dijalankan dalam satu layer...

ketika gagal di baris perintah tertentu, lalu kita build ulang, akan dilanjutkan pada posisi build terakhir yang berhasil...
nah, untuk selanjutnya temen-temen bisa membuild sebuah aplikasi tanpa menghabiskan kuota yang banyak...

bagaimana caranya?

pada hub.docker.com terdapat fasilitas build otomatis... itu terintegrasi dengan github.com

syaratnya apa?

syaratnya temen-temen harus paham menggunakan git,

git adalah sebuah version control system, github.com menggunakan git...

nah, saya pernah membuat sebuah docker image dari sebuah aplikasi bernama OpenSID

dapat diakses di sini : https://github.com/samsulmaarif/opensid-docker

oke, saya kira kulgram kali ini cukup...

apakah ada pertanyaan lagi teman-teman?

lebih kurang saya mohon maaf, jika ada yang kurang berkenan di hati sekali lagi saya mohon maaf...

mohon maaf jika ada penjelasan yang membingungkan, saya sendiri masih belajar...

semoga apa yang kita pelajari malam ini dapat bermanfaat bagi kita semua...

wassalamu'alaikum warohmatullohi wabarokattuh...

### KULGRAM #002 Docker SELESAI ###

