---
layout: post
title: wvdial GUI
date: '2011-11-12T12:40:00.001+07:00'
author: Samsul Maarif
categories: blog
tags:
- Tutorial
modified_time: '2011-11-12T12:51:52.656+07:00'
thumbnail: https://lh5.googleusercontent.com/-0BqXpXCpu_0/Tr4EL3mDqtI/AAAAAAAAAPo/4nwP7AWe5Xo/s72-c/Gambar-Layar-77.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-6203217173574042141
blogger_orig_url: http://blog.samsul.web.id/2011/11/wvdial-gui.html
---

Tersedianya repository offline memancing saya untuk dapat mencoba berbagai aplikasi dengan menginstallnya hanya dengan mengetikkan beberapa perintah di terminal/konsole maupun melalui synaptic. Seperti ketika koneksi internet di warnet saya benar-benar mati, saya mencoba mencari aplikasi di synaptic dengan kata kunci "wvdial". Hasil pencarian nampak seperti yang diharapkan, wvdial berada di posisi paling atas, dan di bawahnya terdapat paket aplikasi bernama gnome-ppp. Secara iseng, saya memilih paket gnome-ppp untuk diinstall sebelum memilih paket wvdial dan ternyata gnome-ppp memiliki dependensi/ketergantungan dengan wvdial. Dan secara otomatis, paket aplikasi wvdial ikut terinstall bersamaan dengan ketika gnome-ppp diinstall.

Berikut ini adalah deskripsi dari gnome-ppp yang juga dapat kita baca di synaptic :

> GNOME PPP is an easy to use graphical dialup connection configuring  
> and dialing tool with system tray icon support.  
>   
> It uses GNOME/GTK+ for its graphical interface and integrates well  
> in GNOME desktop environment, but it can be used in other environments.  
>   
> It also uses WvDial dialer as its backend, providing simple configuration  
> via config files. You can also use plain wvdial if you don't have X running.  
>   
>  Homepage: http://www.gnome-ppp.org/  
> 

Ketika kita masih menggunakan wvdial secara langsung melalui terminal, kita akan familiar dengan beberapa perintah diantaranya "wvdialconf" (untuk mendeteksi modem dan membuat berkas konfigurasi /etc/wvdial.conf) dan "wvdial nama_koneksi" (untuk melalukan dial a.k.a mengoneksikan ke internet), dengan gnome-ppp kita hanya perlu memasukkan beberapa parameter yang diperlukan dan beberapa klik saja.

Untuk menginstall paket aplikasi gnome-ppp melalui terminal ketikkan perintah berikut di terminal :

> $ sudo apt-get install gnome-ppp  
> 

Setelah terinstall, klik menu Aplikasi > Internet > GNOME PPP, penampakannya seperti gambar berikut :

[![](https://lh5.googleusercontent.com/-0BqXpXCpu_0/Tr4EL3mDqtI/AAAAAAAAAPo/4nwP7AWe5Xo/s720/Gambar-Layar-77.png "Gambar-Layar-77")](https://lh5.googleusercontent.com/-0BqXpXCpu_0/Tr4EL3mDqtI/AAAAAAAAAPo/4nwP7AWe5Xo/s720/Gambar-Layar-77.png)

Sebelah kiri adalah penampakan awal gnome-ppp, untuk menampilkan dialog konfigurasinya, klik tombol Setup  yang berada di tengah. Saya rasa dengan GUI seperti ini, konfigurasi modem akan lebih mudah terutama untuk pemula seperti saya, dan lebih "manusiawi". Jika ada yang penasaran dengan penampakan Setup di tab lainnya, berikut ini saya tangkapkan penampakan di mesin BlankOn saya :

[![](https://lh5.googleusercontent.com/-FiGvZ2eDpoo/Tr4EFTmRQwI/AAAAAAAAAPY/d4XBulpiOOA/s475/Gambar-Layar-Setup.png "Gambar-Layar-Setup")](https://lh5.googleusercontent.com/-FiGvZ2eDpoo/Tr4EFTmRQwI/AAAAAAAAAPY/d4XBulpiOOA/s475/Gambar-Layar-Setup.png)

[](http://192.168.2.100/alfa.net/wp-content/uploads/2011/11/Gambar-Layar-Setup.png)Untuk modem, biasanya konfigurasi Internet Protocol (IP) diset dinamis, begitu juga dengan DNS.

![](https://lh6.googleusercontent.com/-Q1gOnBwxuZY/Tr4EGyWg2XI/AAAAAAAAAPg/_2u_dZk28Tk/s475/Gambar-Layar-Setup-1.png "Gambar-Layar-Setup-1")

[](http://192.168.2.100/alfa.net/wp-content/uploads/2011/11/Gambar-Layar-Setup-1.png)Meskipun saya belum mencobanya karena saya tidak memiliki modem, tapi semoga catatan ini dapat bermanfaat buat temen-temen yang membutuhkan.

Tambahan :

Berikut ini adalah alternatif yang pernah saya coba di laptop teman yang sempat berkunjung ke gubug saya. Jika modem kita tidak terdeteksi, misalnya hanya terdeteksi sebagai cdrom (/dev/srX), eject dulu dengan perintah :

> $ sudo eject /dev/srX  
> 

sesuaikan X dengan kondisi di komputer kita. Atau dengan cara yang lebih mudah, melalui nautilus klik kanan pada cdrom kemudian pilih Lepas kaitan (unmount). Kemudian coba kembali melalui setup gnome-ppp klik tombol 'detect'.

Jika masih belum terdeteksi, ketikkan perintah berikut di terminal :

> $ lsusb  
> 

perintah tersebut adalah untuk melihat serial dari usb modem kita. Ketikkan perintah tersebut sebelum dan sesudah ditancapkan, perhatikan perbedaannya. Sebagai misalnya, usb modem milik teman saya terdeteksi dengan 21f5:2008 maka masukkan parameter usbserial di terminal dengan perintah sebagai berikut :

> $ sudo modprobe usbserial vendor=0x21f5 product=0x2008  
> 

Setelah melalui proses ini, biasanya modem akan terdeteksi. Coba kembali klik tombol 'detect'.

Sekian, semoga bermanfaat. Jika Anda menemukan kesalahan pada postingan saya, mari kita diskusikan melalui komentar. Feel free and be happy :D
