---
layout: post
title: Screencast OBS Studio ke Google Meet di openSUSE
date: '2020-04-22'
author: Samsul Maarif
categories: blog
tags:
- Linux
- openSUSE
bigimg:
  - "/img/muktisari-20190612.jpg" : "Kampung Halaman, Desa Muktisari, Gandrungmangu, Cilacap (2019)"
share-img: "/img/muktisari-20190612.jpg"
---

Saat ini masanya, masa pandemi ini banyak yang melakukan pekerjaan dari rumah ("WFH"). Salah satu kebutuhan saat *meeting* secara daring adalah melakukan *screencasting*, atau berbagi layar. Catatan ini menjelaskan bagaimana cara melakukannya dengan OBS Studio dan Google Meet.

OS yang saya gunakan di laptop adalah openSUSE Leap 15.1. Yang perlu dilakukan pertama kali adalah menginstall aplikasinya :

```
sudo zypper install obs-studio
```

Coba buka aplikasi obs studio, aktifkan kamera dan berbagi layar. Misalnya begini:

![Aplikasi OBS Studio](https://i.imgur.com/TOlywpw.png)

Pasang juga beberapa paket lain yang dibutuhkan:

```
sudo zypper install obs-v4l2sink v4l2loopback-utils
```

Kedua paket tersebut diperlukan untuk membuat *kamera virtual*. Kamera virtual tersebut nantinya akan kita pasang di Google Meet.

Selanjutnya kita buat perangkat *loopback* (loopback device) untuk kamera virtual tersebut:

```
sudo modprobe v4l2loopback devices=1 video_nr=10 card_label="OBS Cam" exclusive_caps=1
```

Buka kembali obs studio, lalu buka menu **Tools** -> **v4l2sink** -> seperti gambar berikut

![pengaturan v4l2sink](https://i.imgur.com/IcUlMmX.png)

Masukkan `/dev/video10` lalu pencet tombol **Start**, pastikan tidak ada peringatan berwarna merah.

Buka https://meet.google.com gabung ke ruang meeting atau buat ruang sendiri, lalu buka pengaturan kamera. Pada bagian Video, pilih **OBS Cam** seperti terlihat pada tangkapan layar berikut:

![Pengaturan Kamera di Google Meet](https://i.imgur.com/uQ6fBhA.png)

Hasilnya akan tampak seperti berikut:

![Hasilnya terbalik](https://i.imgur.com/N28pXT8.png)

Jangan khawatir, hasilnya memang terbalik dari sisi kita. Namun jika dilihat dari sisi pengguna lain hasilnya tampak normal. Selamat mencoba.

> **CATATAN**:  
- pada percobaan yang saya lakukan, hasil dari screencasting (apasih ini bahasa Indonesianya yang tepat) lebih suram (nge-blurr) daripada berbagi layar langsung dari menu di Google Meet.  
- Kemungkinan karena faktor koneksi internet yang saya gunakan, namun bisa jadi karena memang ada pengaturan di obs yang belum saya sesuaikan.  
- Terlepas dari hal tersebut, kelebihan menggunakan metode ini adalah kita dapat menyesuaikan **Scene** yang akan kita tampilkan sehingga nampak lebih profesional bak siaran di televisi. #halah

Buat systemd service agar perangkat **loopback** (`/dev/video10`) dibuat secara otomatis pada saat komputer dinyalakan.

```
sudo vim /etc/systemd/system/v4l2loopback.service
```

lalu isikan sebagai berikut:

```
# /etc/systemd/system/v4l2loopback.service
#

[Unit]
Description=Activate v4l2loopback at startup

[Service]
Type=oneshot
ExecStart=/sbin/modprobe v4l2loopback devices=1 video_nr=10 card_label="OBS Cam" exclusive_caps=1

[Install]
WantedBy=multi-user.target

```

aktifkan dengan perintah berikut:

```
sudo systemctl enable v4l2loopback
sudo systemctl status v4l2loopback
```

Dengan begitu kita tidak perlu mengetikkan `modprobe blabla` secara manual agar perangkat (kamera) virtual aktif.

Referensi : [https://srcco.de](https://srcco.de/posts/using-obs-studio-with-v4l2-for-google-hangouts-meet.html)
