---
layout: post
title: Dokumentasi OLSR di Destika 2014
date: '2014-10-03T22:55:00.000+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Access Point
- Ad-Hoc
- olsr
- speedywiki
- olsrd
- Jaringan MESH
modified_time: '2014-10-03T22:55:25.311+07:00'
thumbnail: http://4.bp.blogspot.com/-4_2JtMHkRc8/VC7FTMSQH-I/AAAAAAAADEU/BrYtV1Lj9AI/s72-c/jaringan-mesh-0.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-1265195559003238271
blogger_orig_url: http://blog.samsul.web.id/2014/10/dokumentasi-olsr-di-destika-2014.html
---



Ini adalah dokumentasi membuat jaringan MESH dengan OLSR yang disampaikan pak Onno W. Purbo di acara Festival Desa TIK 2014 di Desa Tanjungsari, Kecamatan Sukahaji, Majalengka, Jawa Barat. Festival tersebut berlangsung selama 2 hari, yaitu pada tanggal 26-27 September 2014\. Salah satu kelas favorit saya adalah kelasnya pak Onno W. Purbo. Ada beberama materi yang beliau sampaikan dalam kelas tersebut, di antaranya yaitu IPv6, Membuat Jaringan WiFi untuk Pedesaan, dan Jaringan MESH dengan OLSR.

Nah, kali ini saya akan menuliskan tentang materi yang terakhir. Peralatan yang dipergunakan kemarin pada saat praktek adalah Router yang telah dikonfigurasi oleh pak Onno untuk bekerja pada mode ad-hoc, dan telah terinstall olsrd dengan sistem operasi OpenWrt. Satu lagi berupa NanoStation yang juga telah terinstall sistem yang sama.

Untuk praktek kali ini, install [OLSR](http://opensource.telkomspeedy.com/wiki/index.php/OLSR "http://opensource.telkomspeedy.com/wiki/index.php/OLSR") di laptop. [Sistem Operasi](http://en.wikipedia.org/wiki/Sistem%20Operasi "http://en.wikipedia.org/wiki/Sistem Operasi") yang saya gunakan di laptop saya adalah Ubuntu Studio 14.04 64 bit dengan repository `trusty-updates` diaktifkan. Sebenarnya bisa diinstall dari repository dengan mengetikkan perintah :

```
[samsul@studio Source]$ sudo aptitude install olsrd olsrd-plugins
```

Namun menurut pak Onno, versi dari repositori sering bermasalah. Jadi sebaiknya compile sendiri dari source. Unduh source code dari[http://www.olsr.org/?q=download](http://www.olsr.org/?q=download "http://www.olsr.org/?q=download"). Oiya, sebelum melakukan kompilasi, install dulu paket ketergantungannya sebagai berikut :

```
[samsul@studio Source]$ sudo aptitude install kernel-package libncurses5-dev fakeroot wget \  
  bzip2 g++ libssl-dev libxml2-dev doxygen bison flex libc6
```

Saya secara personal lebih suka menggunakan `aptitude` ketimbang `apt-get`. Kalau di laptop Anda tidak ada, silahkan diinstall dulu, atau pake saja `apt-get`. Pada saat compile kemarin, saya mengalami kesulitan mengompile versi terkini dari OLSR (versi 0.6.6.2), jadi saya unduh versi `olsrd-0.6.2.tar.bz2`.

Ekstrak dan compile berkas tarbal dengan perintah :

```
[samsul@studio Source]$ tar -xvjf olsrd-0.6.2.tar.bz2  
[samsul@studio Source]$ cd olsrd-0.6.2  
[samsul@studio olsrd-0.6.2]$ make all
```

Selanjutnya install :

```
[samsul@studio olsrd-0.6.2]$ sudo make install
```

Sebelum menjalankan OLSR, edit berkas konfigurasinya dulu :

```
[samsul@studio olsrd-0.6.2]$ sudo cp /etc/olsrd.conf /etc/olsrd.conf.orig  
[samsul@studio olsrd-0.6.2]$ sudo vim /etc/olsrd.conf
```

Pada blok baris paling bawah, pastikan interface yang aktif adalah yang sedang digunakan. Dalam hal ini saya menggunakan `wlan0` :

```
Interface "wlan0"  
{  
    # Interface Mode is used to prevent unnecessary  
    # packet forwarding on switched ethernet interfaces  
    # valid Modes are "mesh" and "ether"  
    # (default is "mesh")  

    # Mode "mesh"  
}
```

Jalankan OLSR dengan :

```
[samsul@studio olsrd-0.6.2]$ sudo olsrd -f /etc/olsrd.conf
```

Nah, kalau muncul error seperti yang saya alami berikut :

---------- LOADING LIBRARY olsrd_txtinfo.so.0.1 ----------  
DL loading failed: "olsrd_txtinfo.so.0.1: cannot open shared object file: No such file or directory"!  
-- PLUGIN LOADING FAILED! --

Hentikan olsr dengan menekan `Ctrl+c` di keyboard, lalu edit berkas konfigurasi tadi (`/etc/olsrd.conf`), dan cari dan beri komentar (pagar) baris :

```
# LoadPlugin "olsrd_txtinfo.so.0.1"  
# {  
    # port number the txtinfo plugin will be listening, default 2006  
#   PlParam     "port"   "81"  
    # ip address that can access the plugin, use "0.0.0.0"  
    # to allow everyone  
#    PlParam     "Accept"   "127.0.0.1"  
#}
```

Hal/solusi tersebut saya temukan setelah gugling sekilas dan menemukan [email ini](https://lists.olsr.org/pipermail/olsr-dev/2011-March/004401.html "https://lists.olsr.org/pipermail/olsr-dev/2011-March/004401.html") di milis Olsr-dev. Setelah itu jalankan kembali olsrd seperti perintah di atas :

```
[samsul@studio olsrd-0.6.2]$ sudo olsrd -f /etc/olsrd.conf
```

Selanjutnya adalah buat SSID baru dengan NetworkManager, caranya klik NetworkManaget pada panel, pilih sunting (di Ubuntu Studio saya ada di paling bawah), lalu  

[![](http://4.bp.blogspot.com/-4_2JtMHkRc8/VC7FTMSQH-I/AAAAAAAADEU/BrYtV1Lj9AI/s1600/jaringan-mesh-0.png)](http://4.bp.blogspot.com/-4_2JtMHkRc8/VC7FTMSQH-I/AAAAAAAADEU/BrYtV1Lj9AI/s1600/jaringan-mesh-0.png)

Klik tambah, lalu pilih WiFi, buat WiFi dengan konfigurasi seperti ini :

[![](http://3.bp.blogspot.com/-2axFpqlO0Hc/VC7FUdK42II/AAAAAAAADEc/NoquHubLFHU/s1600/jaringan-mesh-1.png)](http://3.bp.blogspot.com/-2axFpqlO0Hc/VC7FUdK42II/AAAAAAAADEc/NoquHubLFHU/s1600/jaringan-mesh-1.png)

[![](http://1.bp.blogspot.com/-TOg4mTvBKmA/VC7FSylY15I/AAAAAAAADEQ/HdWHO0N0gRQ/s1600/jaringan-mesh-2.png)](http://1.bp.blogspot.com/-TOg4mTvBKmA/VC7FSylY15I/AAAAAAAADEQ/HdWHO0N0gRQ/s1600/jaringan-mesh-2.png)

Jangan lupa klik tombol Simpan. Selesai, tinggal dites, sambungkan dengan jaringan di sekelilingnya. Oiya, pastikan SSID WiFi yang lainnya sama, tidak harus dengan kata `MESH`. Kira-kira topologinya akan seperti[1)](http://wiki.samsul.web.id/linux/Dokumentasi.OLSR.di.Destika.2014#fn__1) :  

[![](http://2.bp.blogspot.com/-i-YAf1O1weE/VC7FVT59VSI/AAAAAAAADEo/yytK8MdVyF0/s1600/jaringan-mesh-3.jpg)](http://2.bp.blogspot.com/-i-YAf1O1weE/VC7FVT59VSI/AAAAAAAADEo/yytK8MdVyF0/s1600/jaringan-mesh-3.jpg)


OLSR akan melakukan scanning, dan ping secara otomatis. Dan mendeteksi SSID yang terdekat untuk kemudian menggunakannya sebagai relay. Kira-kira demikianlah, semoga tidak membingungkan.

[1)](http://wiki.samsul.web.id/linux/Dokumentasi.OLSR.di.Destika.2014#fnt__1) gambar diambil dari [SpeedyWiki](http://opensource.telkomspeedy.com/wiki/index.php/SpeedyWiki "http://opensource.telkomspeedy.com/wiki/index.php/SpeedyWiki")
