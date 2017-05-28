---
layout: post
title: Memanfaatkan XAMPP Control Panel
date: '2012-11-18T11:55:00.003+07:00'
author: Samsul Maarif
categories: blog
tags: 
modified_time: '2012-11-18T11:55:41.029+07:00'
thumbnail: http://4.bp.blogspot.com/-7yjGaaQ9uf0/UKhZymcj5KI/AAAAAAAAElo/SSxLea_F84o/s72-c/Screenshot+-+161112+-+21:17:30-crop1.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-4231081817308899041
blogger_orig_url: http://blog.samsul.web.id/2012/11/memanfaatkan-xampp-control-panel.html
---

Langsung saja kawan, tak perlu basa-basi. Tulisan ini hanya untuk mendokumentasikan apa yang pernah saya lakukan ketika bermain dengan [GNU/Linux](http://www.gnu.org/gnu/linux-and-gnu.html). Lebih spesifiknya ketika saya bermain Linux di komputer teman, dengan spesifikasi yang minimal yaitu P4 dengan RAM 512 (detilnya tak sempat saya dokumentasikan) pemilik komputer ingin membuat webserver untuk diinstall CMS [joomla](http://www.joomla.org/), [wordpress](http://www.wordpress.org/), dll. Kondisi komputer yang tidak terhubung dengan internet, dan Sistem Operasi yang terinstall adalah [Ubuntu 9.10](https://wiki.ubuntu.com/KarmicKoala) ([Karmic Koala](https://wiki.ubuntu.com/KarmicKoala)), maka saya downloadkan xampp for linux dengan notebook saya lalu diinstall ke komputer teman saya tersebut. {weleh, malah jadi basa-basi. wakwkakw.}  

Seperti yang disebutkan di [website resminya](http://www.apachefriends.org/en/xampp-linux.html#377), proses instalasi xampp di linux sangat mudah. Asumsikan berkas yang didownload bernama xampp-linux-1.8.1.tar.gz dan target instalasinya adalah /opt maka perintah yang diketikkan di terminal adalah :  

$ sudo tar xvfz xampp-linux-1.8.1.tar.gz -C /opt  

Dan, proses instalasi selesai.  

Sekarang tinggal buat launcher XAMPP Control Panel. Program kecil ini telah disertakan dalam paket XAMPP dalam direktori $XAMPPINSTALLDIR/lampp/share/xmpp-control-panel. Oiya, SS berikut saya buat dengan notebook saya dengan sistem operasi Ubuntu Studio 12.04\. Prosesnya tidak jauh berbeda dengan distro-distro lain, termasuk Ubuntu Karmic Koala milik teman saya. Maunya sih gambarnya saya ambil dari komputer teman saya tersebut, namun karena berbagai hal. Termasuk karena ketika melakukan instalasi sedikit terburu-buru dan tidak kepikiran untuk mendokumentasikan.  

Dari README XAMPP Control Panel dijelaskan bahwa ini adalah program kecil yang dapat digunakan untuk mengontrol paket aplikasi pengembangan web XAMPP. Alat ini memungkinkan Anda untuk menjalankan dan menghentikan komponen XAMPP dengan mudah secara grafis.  

Kebutuhan sistem :  

Linux (may work on other OSs)  
GTK 2.x  
PyGTK (also known as python-gtk) 2.x  
XAMPP  

Sekarang kita buat launchernya, klik kanan di desktop {eits, temen-temen enggak akan nemu kata "Refresh" di sini}, pilih Create Launcher :  

[![](http://4.bp.blogspot.com/-7yjGaaQ9uf0/UKhZymcj5KI/AAAAAAAAElo/SSxLea_F84o/s1600/Screenshot+-+161112+-+21:17:30-crop1.png)](http://4.bp.blogspot.com/-7yjGaaQ9uf0/UKhZymcj5KI/AAAAAAAAElo/SSxLea_F84o/s1600/Screenshot+-+161112+-+21:17:30-crop1.png)

Selanjutnya masukkan parameter sebagai berikut :  

[![](http://4.bp.blogspot.com/-FkXwZRoNkds/UKhZ09VspXI/AAAAAAAAElw/UtdkgRMq1zY/s320/Screenshot+-+161112+-+21%253A19%253A57.png)](http://4.bp.blogspot.com/-FkXwZRoNkds/UKhZ09VspXI/AAAAAAAAElw/UtdkgRMq1zY/s1600/Screenshot+-+161112+-+21%253A19%253A57.png)

Name :   
Comment :   
Command :  gksu /opt/lampp/share/xampp-control-panel/xampp-control-panel  
Working Directory :   
Icon :   

penambahan 'gksu' di depan adalah agar kita dapat menjalankan program tersebut dengan hak akses super user. Setelah semuanya sudah sesuai, klik tombol create. Akan muncul ikon di desktop, dobel klik ikon tersebut untuk menjalankannya.  

[![](http://4.bp.blogspot.com/-JhbwJZNMcAs/UKhlVLw2a2I/AAAAAAAAEmA/SOnZhjYkPDc/s1600/Screenshot+-+181112+-+11:27:19-crop1.png)](http://4.bp.blogspot.com/-JhbwJZNMcAs/UKhlVLw2a2I/AAAAAAAAEmA/SOnZhjYkPDc/s1600/Screenshot+-+181112+-+11:27:19-crop1.png)

Jika berhasil akan muncul tampilan sebagai berikut :  

[![](http://1.bp.blogspot.com/-JDyiXe3ol-I/UKhZxU_F8XI/AAAAAAAAElg/Y3jUJz3jAv8/s1600/Screenshot+-+161112+-+21%253A14%253A58.png)](http://1.bp.blogspot.com/-JDyiXe3ol-I/UKhZxU_F8XI/AAAAAAAAElg/Y3jUJz3jAv8/s1600/Screenshot+-+161112+-+21%253A14%253A58.png)

Sekarang kita dapat dengan mudah menjalankan dan menghentikan service Apache, MySQL, maupun ProFTPD bawaan XAMPP for Linux.  

Demikian, semoga bermanfaat.  

Resources :  

*   [apachefriends](http://www.apachefriends.org/)
*   [Ubuntu Wiki](https://wiki.ubuntu.com/)
