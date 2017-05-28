---
layout: post
title: Laptop Access Point non Ad-Hoc
date: '2014-08-25T13:22:00.001+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Access Point
- Linux
- Ad-Hoc
- OpenSource
- Ubuntu
- Software
modified_time: '2014-08-25T13:22:16.628+07:00'
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-809709955530029389
blogger_orig_url: http://blog.samsul.web.id/2014/08/laptop-access-point-non-ad-hoc.html
---

Sejak pertama kali pegang ponsel Android, memang seolah ada perbedaan gaya hidup. Apa yang sebelumnya hanya dapat dilakukan menggunakan laptop/komputer beberapa dapat dilakukan dalam genggaman, seperti membaca & mengirim surel, membaca buku elektronik, bahkan menulis berita. Namun tak jarang, beberapa hal tersebut tidak dapat dilakukan di ponsel karena terkendala koneksi internet.  

Ketika kartu SIM di ponsel masih ada paket internet, mungkin akan mudah. Ponsel Android sudah memfasilitasi, sudah tersedia fitur hotspot tethering maupun maupun via USB. Ini sangat memudahkan terutama ketika saya perlu melakukan pertukaran data dari ponsel ke laptop atau sebaliknya. Ya, bukan lagi menggunakan bluetooth tapi WiFi. Selain itu, ponsel Android dapat pula digunakan sebagai remote baik untuk menggerakkan kursor maupun sebagai papan-tik. Namun bukan ini yang akan saya tuliskan di sini.  

Belakangan saya rasakan, menggunakan ponsel (saya) sebagai modem koneksinya sangat tidak stabil. Kalau dilihat di indikator, sinyalnya sering hilang. Sistem Operasi (laptop; Ubuntu Studio 14.04 LTS) yang saya gunakan juga telah memfasilitasi untuk berbagi koneksi internet. Istilahnya Internet Conection Sharing (Berbagi Sambungan Internet), misalnya laptop sudah terkoneksi melalui jaringan kabel atau modem, saya dapat berbagai koneksi internet ke komputer/laptop melalui WiFi. Atau sebaliknya misalnya jika mendapat koneksi dari WiFi, LAN dapat digunakan untuk berbagi ke komputer lain.  

Secara default, Desktop Ubuntu dapat dibuat AP (Access Point; WiFi) dengan bantuan NetworkManager. Komputer/laptop lain pun dapat terhubung dengannya sebagai sumber koneksi internet maupun hanya untuk bertukar data. Permasalahannya adalah, AP yang dibuat tersebut bertipe Ad-Hoc. Ketika saya ubah menjadi Managed/Infrastructure, penerima (klien) tidak dapat terhubung (ke internet). sedangkan ponsel android standarnya memang [tidak mendukung AP dengan tipe Ad-Hoc](http://code.google.com/p/android/issues/detail?id=82).  

Nah setelah sekian waktu lamanya saya mencari (atau menunggu; karena saya tidak terlalu serius mencarinya), akhirnya saya menemukan skrip karya [Dashamir Hoxha](https://github.com/dashohoxha) berikut ini :  

Skrip tersebut mengotomatisasi apa yang saya harapkan : membuat akses point yang dapat dijangkau oleh ponsel andoid (saya).  

## Cara Menggunakan?

Untuk menggunakan skrip ini caranya cukup sederhana, salin seluruh isi skrip di atas ke dalam teks editor misalnya diberi nama _install_wifi_access_point.sh_, lalu beri akses eksekusi ke berkas/skrip tersebut :  

```
[samsul@studio Source]$ sudo chmod +x install_wifi_access_point.sh
```

Jalankan skrip tersebut dengan mengetikkan perintah :  

```
[samsul@studio Source]$ sudo bash install_wifi_access_point.sh
```

Nah, berikut log terminal saya ketika mengeksekusi perintah tersebut :  

{% gist 5767262 %}

Hanya perlu satu kali menjalankan skrip tadi, karena fungsinya memang hanya untuk menginstall. Setelah terinstall, kita bisa menjalankan perintah seperti yang nampak di akhir log di atas. Pengalaman saya di Ubuntu Studio 14.04, saya berhasil mengoneksikan ponsel Andoid saya ke laptop. Namun seringkali, karena koneksinya kadang kurang stabil saya perlu menjalankan perintah-perintah berikut secara manual :  

```
[samsul@studio Source]$ sudo rfkill unblock wlan  
[samsul@studio Source]$ sudo ip link set dev wlan0 up  
[samsul@studio Source]$ sudo ip address add 10.10.0.1/24 dev wlan0  
[samsul@studio Source]$ sudo iptables -t nat -A POSTROUTING -s 10.10.0.0/16 -o ppp0 -j MASQUERADE  
[samsul@studio Source]$ sudo service isc-dhcp-server start  
isc-dhcp-server start/running, process 4221  
[samsul@studio Source]$ sudo service hostapd start  
 * Starting advanced IEEE 802.11 management hostapd                                             [ OK ]   
[samsul@studio Source]$ ifconfig  
eth0      Link encap:Ethernet  HWaddr dc:0e:a1:5b:de:fe    
          UP BROADCAST MULTICAST  MTU:1500  Metric:1  
          RX packets:5514732 errors:0 dropped:0 overruns:0 frame:0  
          TX packets:4572095 errors:0 dropped:0 overruns:0 carrier:28  
          collisions:0 txqueuelen:1000   
          RX bytes:6218221124 (6.2 GB)  TX bytes:2292538309 (2.2 GB)  

lo        Link encap:Local Loopback    
          inet addr:127.0.0.1  Mask:255.0.0.0  
          inet6 addr: ::1/128 Scope:Host  
          UP LOOPBACK RUNNING  MTU:65536  Metric:1  
          RX packets:1149443 errors:0 dropped:0 overruns:0 frame:0  
          TX packets:1149443 errors:0 dropped:0 overruns:0 carrier:0  
          collisions:0 txqueuelen:0   
          RX bytes:251284768 (251.2 MB)  TX bytes:251284768 (251.2 MB)  

ppp0      Link encap:Point-to-Point Protocol    
          inet addr:10.90.39.178  P-t-P:10.64.64.64  Mask:255.255.255.255  
          UP POINTOPOINT RUNNING NOARP MULTICAST  MTU:1500  Metric:1  
          RX packets:37 errors:0 dropped:0 overruns:0 frame:0  
          TX packets:835 errors:0 dropped:0 overruns:0 carrier:0  
          collisions:0 txqueuelen:3   
          RX bytes:4439 (4.4 KB)  TX bytes:54573 (54.5 KB)  

wlan0     Link encap:Ethernet  HWaddr c0:18:85:5a:94:6f    
          inet addr:10.10.0.1  Bcast:0.0.0.0  Mask:255.255.255.0  
          inet6 addr: fe80::c218:85ff:fe5a:946f/64 Scope:Link  
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1  
          RX packets:91732 errors:0 dropped:0 overruns:0 frame:0  
          TX packets:114252 errors:0 dropped:0 overruns:0 carrier:0  
          collisions:0 txqueuelen:1000   
          RX bytes:41297838 (41.2 MB)  TX bytes:53259798 (53.2 MB)  

[samsul@studio Source]$ 
```

Buat Anda, selamat mencoba.... :-P
