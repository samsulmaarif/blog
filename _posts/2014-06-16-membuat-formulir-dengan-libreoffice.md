---
layout: post
title: Membuat Formulir dengan LibreOffice Base (1)
date: '2014-06-16T03:51:00.000+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Database
- ODBC
- Linux
- Tutorial
- LibreOffice
modified_time: '2014-06-16T03:51:22.392+07:00'
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-8366103816908924358
blogger_orig_url: http://blog.samsul.web.id/2014/06/membuat-formulir-dengan-libreoffice.html
---

Membuat formulir sederhana dengan LibreOffice Base yang kali ini kubuat sebenarnya, kalau kubilang “penak-penak angel” (gampang-gampang susah). Sebelumnya aku pernah beberapa kali secara iseng untuk membuat formulir dengan LibreOffice Base dan pernah sekali berhasil menghubungkannya dengan DBMS MySQL melalui sebuah conector. Hal ini sebenarnya aku terinspirasi oleh guruku ketika praktik TIK semasa SMA. Ketika itu baru menggunakan Ms. Access, guruku mendemonstrasikan pembuatan formulir (form) untuk menginput data siswa. Ada label, ada kotak untuk menginput data, dan ada pula beberapa tombol dengan berbagai fungsi di antaranya untuk menyimpan, untuk membersihkan layar dan lainnya yang masih sangat sederhana. Tak sulit buatku untuk memahami materi saat itu, karena memang sebelumnya aku sudah pernah kursus komputer selama setahun ketika aku masih SMP. Bukan bermaksud untuk berbesar hati, namun setidaknya itu yang kurasakan saat itu.  

Sudah dulu curhatnya. Nah dari hal tersebut, melihat kondisi sekarang yang aku sendiri berusaha untuk konsisten untuk secara terus-menerus menggunakan produk opensource, aku mencoba mengimplementasikan hal tersebut dengan aplikasi yang sehari-hari kugunakan. Dalam hal ini ialah LibreOffice Base.  

Sebenarnya membuat formulir (form) itu yang kita perlukan adalah membuat dulu basisdatanya. LibreOffice Base itu sendiri adalah sebuah aplikasi kantoran yang khusus untuk menangani basisdata. Kita bisa menggunakan engine basisdata bawaan LibreOffice itu sendiri atau menggunakan DBMS lain (misalnya MySQL) dengan menghubungkannya melalui konektor. Kedua hal ini tentu memiliki benefitnya masing-masing. Misalnya kalau kita menggunakan engine bawaan LibreOffice, ini akan menghemat sumberdaya mesin. Sedangkan jika kita menggunakan MySQL sebagai backend basisdatanya, sumberdaya yang diperlukan mungkin akan lebih. Namun keuntungannya kita bisa berbagi basisdata dengan aplikasi lain yang menggunakan MySQL sebagai basisdatanya. Aiih, kok jadi panjang begini penjelasannya. Kalau bingung dilewati saja.  

Oiya, di sini aku tidak akan menjelaskan bagaimana cara menginstall aplikasi yang diperlukan untuk melakukan langkah-langkah yang akan dijabarkan di sini. Sistem operasi yang kugunakan adalah Ubuntu Studio 14.04 x86_64 alias Trusty Tahr yang versi 64 bit. Belum lama ku-upgrade setelah 2 tahun menggunakan versi 12.04 yang 32 bit. Versi LibreOffice yang terinstall ialah versi 4.2.3.3 full install (unduh melalui website resminya/tidak menggunakan repo Ubuntu), sedangkan versi MySQL terinstall ialah versi 5.5.37\. Kalo di laptop/komputermu belum terinstall ya silahkan diinstall dulu.  

Pertama kita perlu memersiapkan konektor LibreOffice ke MySQLnya. Kali ini akan menggunakan ODBC (Open Database Connectivity). Nah, karena iseng aku mencari paket konektor ini dengan perintah 'apt-cache search mysql | grep odbc' yang akhirnya aku menemukan sebuah paket bernama 'libmyodbc'. Kemudian install paket tersebut dengan perintah :  

> sudo aptitude install libmyodbc

Setelah paket tersebut terinstall, kemudian yang kulakukan selanjutnya ialah menyalin berkas '/usr/share/libmyodbc/odbcinst.ini' ke direktori $HOME dengan perintah :  

> cp /usr/share/libmyodbc/odbcinst.ini $HOME/.odbcinst.ini

Selanjutnya buat sebuah berkas .odbc.ini di direktori $HOME :  

> vim $HOME/.odbc.ini

Isikan berkas tersebut seperti konfigurasi sebagai berikut :  

> [Muktisari]  
> Description = Basisdata Muktisari  
> Driver = MySQL  
> Server = localhost  
> Database = muktisari  
> Port = 3306  
> Socket =  
> Option = 3  
> ReadOnly = No 

Keterangan singkatnya begini :  
[Muktisari] → pemberian nama ini bebas, nanti akan muncul sebagai nama datasource ODBC di LibreOffice kita.  
Description → bebas, tidak berpengaruh. Ini hanya berupa penjelasan/keterangan mengenai database kita.  
Database → sesuaikan dengan nama database kita, dalam hal ini nama database yang kugunakan ialah 'muktisari'.  

Nah, sampai di sini tinggal melanjutkan ke LibreOffice Base. Jika terjadi error, silahkan periksa pesan errornya. Tunggu kelanjutan dari tulisan yang belum selesai ini, atau buka tautan referensi di bawah ini...  

**Referensi**  

*   [https://wiki.openoffice.org/wiki/Connect_MySQL_and_Base#Debian.2FUbuntu](https://wiki.openoffice.org/wiki/Connect_MySQL_and_Base#Debian.2FUbuntu)
*   [http://www.easysoft.com/applications/openoffice_org/odbc.html](http://www.easysoft.com/applications/openoffice_org/odbc.html)
*   [libreoffice mysql odbc](https://www.google.com/search?q=libreoffice+odbc+mysql)
