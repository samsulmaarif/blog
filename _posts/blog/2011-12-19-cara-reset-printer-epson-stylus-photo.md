---
layout: post
title: Cara Reset Printer Epson Stylus Photo R230 di Linux (BlankOn)
date: '2011-12-19T14:21:00.003+07:00'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Reset Printer
modified_time: '2011-12-19T15:04:17.574+07:00'
thumbnail: http://4.bp.blogspot.com/-G9--CIN3Hgc/Tu7uP4Jt8ZI/AAAAAAAADgk/0430p7fXgHs/s72-c/Gambar-Layar-Printer%252520Properties%252520-%252520%252527Stylus-Photo-R230%252527%252520on%252520localhost.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-8862368425316319893
blogger_orig_url: http://blog.samsul.web.id/2011/12/cara-reset-printer-epson-stylus-photo.html
---

Printer Epson Stylus Photo R230 termasuk printer yang sering saya gunakan. Ya karena memang printer itu yang disediakan oleh bos saya untuk keperluan mencetak foto. Ketika bos membeli printer baru, beliau tidak membelikan printer dengan merk dan tipe yang baru. Ehtahlah, apakah printer ini dapat dibilang unik atau tidak. Tetapi printer ini memiliki masalah yang spesifik, yaitu terkadang terjadi blink/blank/(atau apa pun itu) yang intinya printer tidak dapat merespon, tidak dapat untuk mencetak yang ditandai dengan kedua lampu merahnya menyala secara bergantian atau bersamaan. Ketika saya menggunakan windows ekspi, saya harus melakukan reset pada printer dengan bantuan software khusus yang dapat dengan mudah didapatkan dengan googling. Tidak percaya? Coba klik [ini](http://www.google.co.id/search?client=opera&rls=id&q=Epson+Stylus+Photo+R230+blink&sourceid=opera&ie=utf-8&oe=utf-8&channel=suggest), [ini](http://www.google.co.id/search?client=opera&rls=id&q=cara+reset+epson+r230&sourceid=opera&ie=utf-8&oe=utf-8&channel=suggest) atau [ini](http://www.google.co.id/search?client=opera&rls=id&q=download+resetter+epson+r230&sourceid=opera&ie=utf-8&oe=utf-8&channel=suggest). Di situ juga dijelaskan bagaimana cara meresetnya. Sangat ribet bukan? Software tersebut bukan buatan dari vendor, namun merupakan buatan pihak ketiga.  

Ok, saya tidak akan menjelaskan panjang kali lebar, alas kali tinggi tentang windows di sini. Dan karena saya sekarang menggunakan BlankOn sebagai OS utama dan satu-satunya di mesin yang saya gunakan untuk bekerja sekarang yang akan saya bahas adalah penyelesaian ketika masalah yang sama terjadi dengan printer Epson Stylus Photo R230 saya.  

Hingga saat ini, printer tersebut termasuk printer yang sangat bersahabat dengan Linux. Istilahnya 'plug and play' sekali dicolokkan di BlankOn, kita langsung dapat menggunakannya untuk mencetak. Printer ini menggunakan driver dari [CUPS (Common Unix Printing System)](http://www.cups.org/) yang kebanyakan distro telah terinstall secara default.  

Suatu ketika saya melakukan cetak dokumen, tiba-tiba printer saya "kumat". [Dua lampunya berkedip-kedip merah](http://www.google.co.id/search?client=opera&rls=id&q=epson+r230+kedip-kedip&sourceid=opera&ie=utf-8&oe=utf-8&channel=suggest). Secara spontan dan refleks saya mencoba mengetikkan perintah berikut di terminal (karena sudah terbiasa melakukan restart service apache "$ sudo /etc/init.d/apache2 restart") :  

> $ sudo /etc/init.d/cups restart

Lalu printer saya matikan dan nyalakan lagi. Viollaa... printer Epson Stylus Photo R230 saya kini sembuh seperti sedia kala (tidak berkedip-kedip lagi). Printer lalu melakukan head cleaning dengan sendirinya.  







[![](http://4.bp.blogspot.com/-G9--CIN3Hgc/Tu7uP4Jt8ZI/AAAAAAAADgk/0430p7fXgHs/s640/Gambar-Layar-Printer%252520Properties%252520-%252520%252527Stylus-Photo-R230%252527%252520on%252520localhost.png)](http://4.bp.blogspot.com/-G9--CIN3Hgc/Tu7uP4Jt8ZI/AAAAAAAADgk/0430p7fXgHs/s695/Gambar-Layar-Printer%252520Properties%252520-%252520%252527Stylus-Photo-R230%252527%252520on%252520localhost.png)





[gambar : Gambar-Layar-Printer Properties - 'Stylus-Photo-R230' on localhost]







Karena merasa belum yakin dengan hasil head cleaning otomatis, saya coba dengan langkah berikut : Sistem > Administrasi > Mencetak (Printing), lalu klik kanan pada printer > pilih Properti > klik tombol "Bersihkan Head Cetak", lalu tunggu beberapa saat. Untuk melihat/menguji hasilnya, klik tombol "Cetak Halaman Uji-sendiri", periksa apakah warnanya sudah lengkap? ulangi langkah tadi hingga warnanya lengkap.  

**[Mohon perhatikan :](http://www.samsul.web.id/p/lisensi.html)**  
Langkah ini dapat berhasil Anda praktekkan hanya jika printer Anda juga menggunakan driver dari cups. Saya menggunaka [BlankOn](http://www.blankonlinux.or.id/) yang antarmuka penggunanya telah menggunakan Bahasa Indonesia, jadi mungkin nama-nama tombol dan menunya agak berbeda jika Anda menggunakan bahasa selain Bahasa Indonesia. :D
