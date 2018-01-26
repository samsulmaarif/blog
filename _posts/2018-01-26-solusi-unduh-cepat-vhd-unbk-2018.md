---
layout: post
title: Solusi Unduh Cepat VHD UNBK 2018 (Sebuah Usulan)
date: '2018-01-26'
author: Samsul Maarif
categories: blog
tags:
- Torrent
- Download
- VHD UNBK 2018
- Unduh Cepat
bigimg: 
  - "/img/kampung-warna-warni-malang_20180117.jpg" : "Kampung Warna-warni Jodipan, Kab. Malang (2018)"
share-img: "/img/kampung-warna-warni-malang_20180117.jpg"
---

Tahun 2018 ini, sebanyak 58.077 sekolah masing-masing 17.268 SMP, 10.208 MTs, 11.191 SMA, 6.905 MA, dan 12.505 SMK (data per 26 Januari 2018, 09:23:50 dari web resmi [unbk.kemdikbud.go.id](https://unbk.kemdikbud.go.id)) kembali menyelenggarakan UNBK. Apa itu UNBK? Berikut saya kutip dari web yang sama :

> Ujian Nasional Berbasis Komputer (UNBK) disebut juga Computer Based Test (CBT) adalah sistem pelaksanaan ujian nasional dengan menggunakan komputer sebagai media ujiannya. Dalam pelaksanaannya, UNBK berbeda dengan sistem ujian nasional berbasis kertas atau Paper Based Test (PBT) yang selama ini sudah berjalan.

Bagaimana teknisnya? Masih saya kutipkan dari laman [yang sama](https://unbk.kemdikbud.go.id/tentang) dijelaskan singkatnya begini:

> Penyelenggaraan UNBK saat ini menggunakan sistem semi-online yaitu soal dikirim dari server pusat secara online melalui jaringan (sinkronisasi) ke server lokal (sekolah), kemudian ujian siswa dilayani oleh server lokal (sekolah) secara offline. Selanjutnya hasil ujian dikirim kembali dari server lokal (sekolah) ke server pusat secara online (upload).

|![](/img/skema-UNBK.png)|
|**Skema UNBK** sumber : unbk.kemdikbud.go.id |

Adalah tugas proktor maupun teknisi (atau proknis; proktor sekaligus teknisi, dan sebaliknya) masing-masing sekolah untuk melakukan unduh berkas [VHD (Virtual Hard Disk)](https://en.wikipedia.org/wiki/VHD_(file_format)). Berkas tersebut berisi sebuah sistem operasi (Windows Server) lengkap dengan aplikasi UNBK-nya. Berkas tersebut akan dijalankan dalam aplikasi VirtualBox yang juga harus dijalankan pada [host yang berbasis OS Windows juga](http://wiki.smktikwt.sch.id/doku.php/UNBK/Server_UNBK_dengan_Linux_Mint_18.1).

Berkas tersebut tidak tanggung-tanggung ukurannya. Tahun lalu adalah 15GB, dipecah menjadi beberapa berkas. Tahun ini, saya lihat postingan seorang teman guru saya di fesbuk ukurannya adalah 20GB. Nah, ini. Yang lucu adalah distribusi berkasnya. Admin pusat menyediakan beberapa server unduh (ftp server). 

Dari yang saya tahu ada beberapa skenario untuk mendapatkan berkas tersebut :

1. Helpdesk (koordinator) provinsi/kabupaten mengunduh berkas tersebut, masing-masing sekolah menyalin (copy via hardisk eksternal) ke helpdesk provinsi/kabupaten (offline, harus ketemu orangnya). Kendalanya apa? bisa antara sekolah dan helpdesk kabupaten jaraknya puluhan kilometer, bahkan ratusan. Sekolah tidak dapat menjangkau/terlalu lama, unduh sendiri juga tidak memungkinkan. Selain karena bandwidth/koneksi di sekolah yang masih minim, server pusat sering overload (akibatnya unduh lelet).
2. Sekolah punya bandwidth & koneksi yang cepet. Tetapi server pusat kolaps karena bandwidth dan koneksi server pusat juga terbatas. Kendatipun ada beberapa server unduh, tapi dengan ribuat sekolah yang saya sebutkan di atas mengunduh bersamaan?
3. Ada sekolah yang telah berhasil mengunduh, berinisiatif menguploadnya ke layanan cloud, google drive misalnya. Sekolah-sekolah lain mengunduh dari link yang diberikan melalui layanan cloud tersebut. Apakah ini menyelesaikan masalah? ternyata tidak juga. Bandwith unduh dari google drive juga dibatasi, mengingat berkas VHD yang ukurannya fantastis.
4. Apa lagi ya... 

Oke, bagi teman-teman proknis pasti merasakan bagaimana perjuangan mendapatkan berkas tersebut agar sekolah dapat melaksanakan UNBK. Jadi, apa saja kendalanya? 

1. Ukuran berkas sangat besar;
2. Bandwith Server unduh terbatas;
3. Akibatnya kecepatan unduh masing-masing sekolah juga terbatas;
4. Hasil unduh rentan korup karena koneksi sering putus, kalo korup? harus unduh ulang;
5. Teman-teman ada yang mau nambahain?

Nah, sebenarnya ada solusi yang menurut saya (sekali lagi: **MENURUT SAYA**) lebih tepat untuk diimplementasikan untuk distribusi berkas *gajah* VHD ini dengan cepat, efektif, dan hemat bandwidth. Solusinya adalah P2P alias **peer-top-peer file sharing** menggunakan protokol **BitTorrent**.

**BitTorrent** merupakan protokol komunikasi berbagi pakai peer-to-peer yang digunakan untuk mendistribusikan data dan berkas elektronik melalui internet. 

Untuk mengirim atau menerima berkas, seseorang menggunakan BitTorrent klien pada komputer yang terhubung ke internet. BitTorrent klien merupakan aplikasi komputer yang mengimplementasikan protokol BitTorrent. Ada banyak sekali BitTorrent klien, di antaranya yang populer adalah  μTorrent, Xunlei, Transmission, qBittorrent, Vuze, Deluge, BitComet, Tixati, dll.

## Manfaat pake BitTorrent? ##

Ada beberapa manfaat yang didapatkan jika menggunakan BitTorrent antara lain:

1. Bandwith server unduh tidak akan overload (kehabisan bandwidth);
2. Sekolah/klien/pengunduh akan mendapatkan bandwith unduh yang lebih cepat;
3. Integritas data yang ditransfer juga terjamin, karena torrent akan secara otomatis melakukan checksum (kriptografi hash) data yang diunduh;
4. Ini adalah **gotong-royong data** yang sesungguhnya, di mana pengunduh yang telah memiliki data akan '*membantu*' pengunduh lain yang belum mendapatkan data tersebut.

## Cara Kerja BitTorrent ##

Jadi seperti apa sih cara kerja BitTorrent kok bisa secanggih itu? 

Sebenarnya ini bukan teknologi baru. Protokol BitTorrent didesain oleh seorang programmer bernama [Bram Cohen](https://en.wikipedia.org/wiki/Bram_Cohen), mantan mahasiswa State University of New York at Bufallo pada April 2001. Pertama kali dirilis pada 2 Juli 2001, dan versi terkini pada tahun 2013.

Nah, berikut gambaran proses transfer data dengan protokol BitTorrent :

|![](https://upload.wikimedia.org/wikipedia/commons/3/3d/Torrentcomp_small.gif)|
|**Gambaran penggunaan protokol BitTorrent** sumber : wikipedia.org|

> **Keterangan gambar** : titik berwarna di bawah komputer pada animasi tersebut merepresentasikan pecahan berkas yang sedang dibagikan. Pada saatnya, salinan pecahan berkas sampai ke komputer tujuan sudah lengkap, penyalinan ke komputer tujuan lain berlangsung antaruser (tidak langsung ke komputer server). Tracker (server) hanya menyediakan salinan tunggal berkas, dan dan pengguna/komputer (pengunduh) mendapatkan salinan secara utuh bagian-bagiannya (pecahannya) dari komputer lain.

Protokol BitTorrent dapat digunakan untuk mengurangi beban server dan jaringan saat mendistribusikan berkas yang besar. Daripada mengunduh berkas dari server sumber tunggal, protokol BitTorrent memungkinkan pengguna (klien/pengunduh) untuk bergabung dengan "swarm" host untuk mengunggah (upload) / mengunduh (download) satu sama lain secara bersamaan. Protokol ini merupakan alternatif dari sumber tunggal yang lebih tua, teknik distribusi data dengan banyak sumber, dan dapat bekerja dengan efektif pada jaringan dengan bandwidth yang rendahh.

|![](https://upload.wikimedia.org/wikipedia/commons/0/09/BitTorrent_network.svg)|
|**Gotong royong data** sumber : wikipedia.org|

> **Keterangan gambar** : komputer yang di tengah berrtindak sebagai "benih" untuk menyediakan berkas ke komputer lain yang bertindak sebagai "peer" (rekan).

Menggunakan protokol BitTorrent, beberapa komputer dasar, seperti komputer rumahan, dapat menggantikan server besar saat mendistribusikan berkas ke banyak penerima secara efektif. Seorang pengguna yang ingin mengunggah sebuah berkas, pertama dia membuat sebuah berkas *desktiptor torrent* yang kemudian berkas desktiptor (berupa berkas \*.torrent) tersebut didistribusikan secara konvensional (web, email, dll). 

Kemudian berkas yang diunggah dibuat tersedia melalui node BitTorrent dan bertindak sebagai *bibit* (seed). Mereka yang memiliki berkas desktiptor torrent dapat memberikannya ke node BitTorrent mereka sendiri, yang bertindak --sebagai rekan sebaya (peer) atau *leechers*-- mengunduhnya dengan menghubungkan ke benih (seed) dan / atau rekan (peer) lainnya.

Berkas yang sedang didistribusikan dibagi dalam segmen yang disebut pecahan (*pieces*). Setiap peer menerima pecahan baruu dari sebuahh berkas, dan menjadi sumber (pecahan tersebut) untuk peer lainnya, jadi benih asli tidak perlu mengirimkan potongan ke setiap komputer atau pengguna yang menginginkan salinannya. 

Dengan BitTorrent, tugas mendistribusikan berkas dibagi oleh mereka yang menginginkannya; sangat mungkin benih tersebut hanya mengirimkan satu salinan berkas itu sendiri dan akhirnya mendistribusikannya ke sejumlah rekan (peer) yang tak terbatas.

Setiap pecahan dilindungi oleh hash kriptografi yang terdapat dalam desktiptor torrent. Hal ini memastikan bahwa setiap modifikasi potongan dapat dideteksi dengan andal, dan karenanya mencegah modifikasi yang tidak disengaja dan berbahaya dari potongan yang diterima node lain. Jika sebuah node dimulai dengan salinan deskriptor torrent yang asli, ia dapat memverifikasi keaslian keseluruhan berkas yang diterimanya.

Nah, kira-kira begitu gambaran protokol BitTorrent. Apakah saya pernah menggunakannya? sering. Kapan? saat mengunduh berkas \*.iso Linux. Pengembang Distro Linux populer biasanya menyediakan (dan menyarankan) unduhan dengan metode **Torrent**. Selain menghemat bandwidth server mereka, mengunduh dengan Torrent bagi pengguna juga lebih cepat (selama tersedia banyak *seeder*). 

## Usulan.....? ## 

Ah, saya cuma rakyat biasa saja yang sedang menuliskan unek-unek saya. Melihat dari tahun-ke tahun gitu-gitu aja. Akhirnya saya geregetan untuk nulis ini. Apakah usulan seperti ini akan didenga/dibaca? Ah, itu urusan nanti. Kalau, kalau nih, misalnya ini diimplementasikan, lalu bagaimana? ya harus ada aturan mainnya. Misalnya? Ya misalnya begini deh, ini yang saat ini ada di pikiran saya :

1. Sekolah/klien/pengunduh harus mengunduh berkas desktiptor torrent (berkas \*.torrent) dari web resmi UNBK;
2. \~\~\~ harus tetap menghidupkan aplikasi BitTorrent klien selama mungkin, minimal selama masa tertentu. Tujuannya tentu saja untuk membantu klien lain yang sedang mengunduh agar dapat mengunduh data dari yang terdekat.
3. Itu dulu sih, nanti saya tambahin kalo kepikiran lagi hehehehehe.

Jadi itulah usul saya kepada admin UNBK pusat (puspendik / atau apalah namanya), yang bertugas mendistribusikan berkas VHD UNBK 2019.

> Buat siapa pun yang membaca tulisan ini, mohon dikoreksi jika terdapat kesalahan/kekeliruan/atau informasi yang salah pada tulisan ini. Komentar dan masukan teman-teman sangat saya apresiasi. Terima kasih.

## Pranala Menarik ##

1. [Server UNBK dengan Linux Mint 18.1](http://wiki.smktikwt.sch.id/doku.php/UNBK/Server_UNBK_dengan_Linux_Mint_18.1)
2. [BitTorrent on WikiPedia ](https://en.wikipedia.org/wiki/BitTorrent)
3. [Perbandingan Klien BitTorrent](https://en.wikipedia.org/wiki/Comparison_of_BitTorrent_clients)
