---
layout: post
title: Cara Mudah Print F4 di Ubuntu
date: '2014-09-10T04:06:00.000+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Tutorial
- OpenSource
- Print F4
- Ubuntu
- Reset Printer
- Canon IP 2700
- Ubuntustudio
modified_time: '2014-09-10T04:58:29.983+07:00'
thumbnail: http://3.bp.blogspot.com/-GKMxn-JoCL4/VA9qVkO7BHI/AAAAAAAADBE/2X_ENCjXZBg/s72-c/print-f4-ubuntu-1.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-3148456263150284447
blogger_orig_url: http://blog.samsul.web.id/2014/09/cara-mudah-print-f4-di-ubuntu.html
---



Salah satu kemudahan menggunakan sistem operasi GNU/Linux adalah `out of the box`. Banyak perangkat keras (hardware) yang kompatibel dan siap pakai ketika menggunakan Linux. Misalnya printer Canon IP 2770 yang kini banyak dipakai di perkantoran. Ketika printer ini dicolokkan ke laptop dengan Sistem Operasi Ubuntu (Studio; yang saya gunakan) 14.04, maka printer akan langsung dapat digunakan. Tinggal dikonfigurasi sesuai dengan kebutuhan.

Nah, permasalahan yang kadang timbul ketika mencetak dokumen PDF dengan `evince` (paket bawaan untuk membuka berkas pdf di Ubuntu Studio saya), adalah printer tidak dapat mencetak dokumen yang diminta atau tercetak tetapi hanya berukuran A4 sedangkan dokumen kita berukuran F4 (21,5 × 33 cm). Namun tak usah khawatir, kali ini saya telah menemukan caranya yang akan saya tuliskan di sini.

Untuk praktek mencetak kali ini saya menggunakan peramban web `google-chrome`. Peramban ini tidak terinstall secara default di Ubuntu, Anda harus menginstallnya secara manual. Caranya, pertama klik kanan pada berkas yang akan dibuka[1)](https://www.blogger.com/blogger.g?blogID=1383767785423682157#fn__1), kemudian pilih `Buka Dengan` (Open with; kalau Anda menggunakan tampilan dalam bahasa Inggris), selanjutnya `Buka dengan Aplikasi lain` seperti gambar berikut:

[![](http://3.bp.blogspot.com/-GKMxn-JoCL4/VA9qVkO7BHI/AAAAAAAADBE/2X_ENCjXZBg/s1600/print-f4-ubuntu-1.png)](http://3.bp.blogspot.com/-GKMxn-JoCL4/VA9qVkO7BHI/AAAAAAAADBE/2X_ENCjXZBg/s1600/print-f4-ubuntu-1.png)

Selanjutnya akan muncul jendela sebagai berikut:  

[![](http://4.bp.blogspot.com/-j2slF8yrmyM/VA9qVasw_AI/AAAAAAAADBI/HoAW6MHv3Gk/s1600/print-f4-ubuntu-2.png)](http://4.bp.blogspot.com/-j2slF8yrmyM/VA9qVasw_AI/AAAAAAAADBI/HoAW6MHv3Gk/s1600/print-f4-ubuntu-2.png)



Cari aplikasi `Google Chrome`[2)](https://www.blogger.com/blogger.g?blogID=1383767785423682157#fn__2), atau dapat pula menggunakan cara yang saya gunakan yaitu dengan menggunakan perintah suai. Seperti terlihat di gambar tersebut saya memasukkan `/usr/bin/google-chrome`. Tekan tombol `Buka`.  

[![](http://4.bp.blogspot.com/-F0L9jEIvZTU/VA9qVyQ5G_I/AAAAAAAADBM/MDZH-tKgtIw/s1600/print-f4-ubuntu-3.png)](http://4.bp.blogspot.com/-F0L9jEIvZTU/VA9qVyQ5G_I/AAAAAAAADBM/MDZH-tKgtIw/s1600/print-f4-ubuntu-3.png)



Pada peramban Google Chrome, menu terletak pada posisi paling kanan sejajar dengan bilah alamat. Klik menu tersebut, lalu klik `Cetak` atau dapat pula dengan menggunakan pitasan `Ctrl+P`.  

[![](http://4.bp.blogspot.com/-WcC4uJ5nVxc/VA9qXHY5fDI/AAAAAAAADBc/JLkVDvO4Y9M/s1600/print-f4-ubuntu-4.png)](http://4.bp.blogspot.com/-WcC4uJ5nVxc/VA9qXHY5fDI/AAAAAAAADBc/JLkVDvO4Y9M/s1600/print-f4-ubuntu-4.png)



Sampai di sini, kita sudah dapat mencetak. Namun ketika saya coba, hasilnya masih belum memuaskan, karena hanya dapat mencetak ukuran A4\. Sekarang klik pada `Cetak menggunakan dialog sistem`, seperti terlihat di sana, Anda juga dapat menggunakan pintasan alias shortcut`Ctrl+Shift+P`. Akan muncul dialog sebagai berikut:  

[![](http://2.bp.blogspot.com/-XYREX9HH3D8/VA9qXg36FyI/AAAAAAAADBg/CkcPreLXpKg/s1600/print-f4-ubuntu-5.png)](http://2.bp.blogspot.com/-XYREX9HH3D8/VA9qXg36FyI/AAAAAAAADBg/CkcPreLXpKg/s1600/print-f4-ubuntu-5.png)



Ups, kok beda! Sebenarnya bukan beda, kalo kita baru pertama kali[3)](https://www.blogger.com/blogger.g?blogID=1383767785423682157#fn__3) membukanya memang akan menampilkan daftar pencetak (printer). Tapi coba perhatikan tab bagian atas, kalau tab yang aktif pada sistem Anda adalah `Umum` ya tentu lain. Coba klik tab `Atur Halaman`, di situ akan kita konfigurasi ukuran dan orientasi.

Nah, karena saya mencetak dokumen dengan ukuran F4 yang ternyata sama dengan `American foolscap` konfigurasinya jadi persis seperti yang tampak pada gambar tersebut. Yang diubah hanya ukuran kertas dan orientasi. Selesai, tinggal klik tombol `Cetak`.  

Oiya, tentu saja tulisan ini tidak hanya berlaku untuk ubuntu saja. Terutama karena menggunakan peramban Google Chrome yang sifatnya cross platform, dapat dicoba di sistem operasi maupun distro lainnya. Pada intinya sebenarnya terletak pada kesesuaian konfigurasi dan niat kita menggunakan sistem operasi yang kita sukai. Dan tentu saja, semua persoalan pasti ada solusinya, seperti halnya setiap penyakit pun ada obatnya, entah sudah ditemukan atau belum. Selamat dan sukses buat Anda pembaca tulisan yang tidak teratur ini.





[1)](https://www.blogger.com/blogger.g?blogID=1383767785423682157#fnt__1) Manajer berkas yang saya gunakan adalah thunar; bawaan Ubuntu Studio, dengan manajer berkas yang berbeda mungkin menunya juga akan berbeda.

[2)](https://www.blogger.com/blogger.g?blogID=1383767785423682157#fnt__2) Google Chrome loh ya, bukan Chromium. Karena Chromium secara _Default_ tidak dapat membuka berkas PDF

[3)](https://www.blogger.com/blogger.g?blogID=1383767785423682157#fnt__3) Setiap mencetak


