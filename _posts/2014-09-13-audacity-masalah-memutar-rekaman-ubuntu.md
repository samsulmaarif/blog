---
layout: post
title: Audacity Masalah Memutar Rekaman Ubuntu 14.04
date: '2014-09-13T03:29:00.000+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Linux
- synaptic
- Tutorial
- OpenSource
- Audacity
- Ubuntu
- FFMpeg
- libavcodec
- Ubuntustudio
modified_time: '2014-09-13T03:40:28.719+07:00'
thumbnail: http://1.bp.blogspot.com/-_wjAmDi0JaQ/VBNPT5w5U2I/AAAAAAAADB4/47L4yjwoyYo/s72-c/update-audacity-1.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-2073164080935745295
blogger_orig_url: http://blog.samsul.web.id/2014/09/audacity-masalah-memutar-rekaman-ubuntu.html
---

Belakangan saya suka merekam dengan ponsel sesuatu yang nampaknya tidak terlalu penting. Terkadang saya memutarnya langsung di ponsel, sesekali saya putar dengan audacity. Tapi ternyata pada Ubuntu Studio 14.04 jika menggunakan Audacity bawaan instalasi tidak dapat digunakan untuk memutar berkas berekstensi *.amr tersebut. Setelah saya coba gugling, akhirnya saya menemukan solusinya.  

Penyebabnya ternyata adalah karena paket audacity untuk Ubuntu [tidak dikompilasi dengan dukungan FFMpeg](http://ubuntuforums.org/showthread.php?t=2218803). Bagaimana mengatasinya? Well, [thanks to Ubuntu developer](https://bugs.launchpad.net/ubuntu/+source/audacity/+bug/1076928). Saya hanya perlu memperbaharui versi audacity dengan terlebih dahulu mengaktifkan repository `trusty-updates`.  

Buka Manajer Paket Synaptic, klik menu `pengaturan` --> `repositori` --> klik tab `pemutakhiran` :  

[![](http://1.bp.blogspot.com/-_wjAmDi0JaQ/VBNPT5w5U2I/AAAAAAAADB4/47L4yjwoyYo/s640/update-audacity-1.png)](http://1.bp.blogspot.com/-_wjAmDi0JaQ/VBNPT5w5U2I/AAAAAAAADB4/47L4yjwoyYo/s1600/update-audacity-1.png)

Selanjutnya reload synaptic dengan menekan tombol `Muat ulang`. Cara tersebut juga dapat dilakukan dengan mengetikkan perintah berikut di terminal :  

$ sudo vim /etc/apt/sources.list

Tambahkan baris berikut pada berkas tersebut :  

deb http://kambing.ui.ac.id/ubuntu/ trusty-updates main restricted universe multiverse

Reload basisdata APT dengan mengetikkan perintah :

$ sudo aptitude update

Nah, selanjutnya (jika proses update telah selesai) tinggal menginstall audacity versi terbaru. Oiya, saya lupa menyebutkan versi yang bermasalah adalah `2.0.5-1ubuntu3`. Dan saya akan menginstall versi `2.0.5-1ubuntu3.2` yang merupakan versi perbaikan dari yang sebelumnya (lihat; perbedaan pada *.2 di bagian akhir). Install audacity dengan perintah :

$ sudo aptitude install audacity

Atau jika menggunakan Synaptic ketikkan `audacity` pada kotak pencarian lalu tekan ENTER. Setelah nampak paket audacity akan nampak bahwa telah tersedia versi terbarunya. Klik kanan, pilih tandai untuk diperbarui, klik tombol `Terapkan`, klik tombol `Terapkan` lagi pada dialog yang muncul untuk mengonfirmasi instalasi.  

[![](http://1.bp.blogspot.com/-dt5GOiaaB4M/VBNVv6tHHaI/AAAAAAAADCE/YztdylvdCR0/s640/update-audacity-4.png)](http://1.bp.blogspot.com/-dt5GOiaaB4M/VBNVv6tHHaI/AAAAAAAADCE/YztdylvdCR0/s1600/update-audacity-4.png)

Akhirnya audacity saya kini sudah dapat untuk mengimpor berkas *.amr (dan berkas dengan format lainnya, *.ac3, *.wma, *.m4a, *.opus, ...). Catatan: selama proses update sebaiknya tutup dulu aplikasi yang akan diupdate. Hal ini untuk menghidari crash pada aplikasi maupun data yang sedang diproses. Sekian catatan saya kali ini. Semoga bermanfaat.
