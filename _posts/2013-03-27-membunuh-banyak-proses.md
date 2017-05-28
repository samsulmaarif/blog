---
layout: post
title: Membunuh Banyak Proses
date: '2013-03-27T02:35:00.001+07:00'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Tutorial
modified_time: '2013-03-27T02:35:18.696+07:00'
thumbnail: http://lh4.ggpht.com/-nsBswFssrE0/UVHnbn0sDjI/AAAAAAAAE7I/DO-Lr2RvU28/s72-c/%25255BUNSET%25255D.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-7693622201341393411
blogger_orig_url: http://blog.samsul.web.id/2013/03/membunuh-banyak-proses.html
---

Adalah saat yang menyebalkan ketika baru saja menyalakan komputer/laptop/notebook, menyolokkan modem, lalu tiba-tiba system load processor menjadi penuh (100%) padahal baru membuka 1 browser. Hal ini saya alami ketika meminjam modem HUAWEI milik teman. Dalam mesin saya telah terinstall Mobile Partner bawaan modem smartfren. Okey, kali ini saya tidak akan membahas masalah modemnya, saya hanya akan membahas bagaimana cara 'membunuh' banyak proses dalam satu barus perintah. 

Ketika modem dicolokkan, hal ini memicu skrip `/sbin/startMobilePartner` untuk dieksekusi. Dalam kasus saya, proses ini terjadi secara berlebihan. Hingga saya memutuskan untuk membuang mode executable-nya dengan menuliskan perintah `sudo chmod -x /sbin/startMobilePartner` agar skrip ini tidak dieksekusi di kemudian hari.

![](http://lh4.ggpht.com/-nsBswFssrE0/UVHnbn0sDjI/AAAAAAAAE7I/DO-Lr2RvU28/%25255BUNSET%25255D.png)

Beberapa menit awal saya kurang menyadari apa yang terjadi dengan mesin/notebook saya. Lalu saya terpikir untuk melihat proses apa saja yang `memakan` prosesor saya saat itu. Saya coba lihat dengan [XFCE Task manager](http://goodies.xfce.org/projects/applications/xfce4-taskmanager) (distro yang saya gunakan [Ubuntu Studio](http://ubuntustudio.org) [12.04](https://help.ubuntu.com/12.04/), dengan DE [XFCE](http://xfce.org)), eh, ternyata dia kolaps juga alias tak merespon hingga harus saya bunuh dengan paksa. Lalu saya lihat dengan [htop](http://htop.sourceforge.net/), dari sini ketahuan tersangkanya adalah `/sbin/startMobilePartner`. Lalu saya gunakan perintah berikut untuk menyimpan proses beserta PIDnya ([Process ID](http://en.wikipedia.org/wiki/Process_identifier)) ke dalam bentuk teks :

```bash
ps ax | grep MobilePartner > /tmp/pid.txt
```

Sekarang saya sudah mendapatkan proses yang jadi `tersangkanya`. Selanjutnya saya harus mencari cara bagaimana dari [file](http://tempel.blankon.in/95117) tersebut saya hanya mengambil PIDnya saja. Saya bertanya pada mbah [google](https://www.google.co.id/search?q=parse+integer+from+text+with+bash&sugexp=chrome,mod=14&sourceid=chrome&ie=UTF-8) sejenak (untungnya masih bisa buat browsingan), dalam satu klik saya menemukan solusinya di [stackoverflow](http://stackoverflow.com/questions/3045493/parse-string-with-bash-and-extract-number). Akhirnya saya simpulkan perintah yang akan saya gunakan adalah sebagai berikut :

```
cd /tmp

awk '{print $1}' pid.txt | cut -d, -f 1
```

Perintah tersebut menghasilkan daftar PID yang akan `dibunuh` dengan perintah `kill` (pid.txt adalah nama berkas yang dibuat sebelumnya) : 

![Process ID yang akan dikill](http://lh4.ggpht.com/-L4aEUxnHFII/UVHy5xAvYaI/AAAAAAAAE7Y/EqeDzDJKmgA/%25255BUNSET%25255D.png)

Sekarang saatnya mengeksekusi perintah `kill -9 pid` yang lengkapnya adalah sebagai berikut :

```
sudo kill -9 $(awk '{print $1}' pid.txt | cut -d, -f 1)
```

![Proses berhasil dibunuh](http://lh5.ggpht.com/-V-BMnHg4F84/UVH23miZRLI/AAAAAAAAE7g/_jOE0RZdz3A/%25255BUNSET%25255D.png)  
Pesan `kill: No such process` mungkin karena prosesnya sudah mati, terlihat ketika saya periksa di `htop` proses yang saya sebutkan di awal sudah tidak ada lagi. 

Alhamdulillah, kini saya dapat kembali bekerja/bermain dengan nyaman tanpa perlu merestart sistem operasi/notebook yang sedang saya gunakan. It's really fun.
