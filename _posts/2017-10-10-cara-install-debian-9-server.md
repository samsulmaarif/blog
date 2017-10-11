---
layout: post
title: Cara Install Debian 9 (Stretch) dengan Pemartisian Manual
date: '2017-10-10'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Debian
- Instalasi
bigimg: 
  - "/img/debian-9/install-debian-42.png" : ""
share-img: "/img/debian-9/install-debian-42.png"
---

Instalasi sistem operasi Debian pada umumnya sama, versi berapa pun. Dan tutorial/panduan instalasi Debian sudah banyak tersebar di internet. Anda dapat dengan mudah mendapatkannya dalam sekejap melalui mesin pencari google. Lalu apa istimewanya tutorial yang saya tulis ini? 

Hehehe... tidak ada yang istimewa kok. Catatan (bukan tutotial loh yah) saya ini hanya menjelaskan bagaimana melakukan instalasi dengan pemartisian secara manual. Karena kebanyakan tutorial installasi Debian adalah dengan pemartisian otomatis. Nah, pada catatan ini akan dipraktekkan cara menginstall Debian 9 (Stretch) dengan pemartisian otomatis. 

Oke, tidak perlu panjang lebar. Contoh kasusnya begini :

> Anda diminta untuk menginstall sebuah server dengan ketentuan :
> <br>1. Buat partisi 10GB dan kaitkan ke **/** (root partition)
> <br>2. Pastikan ukuran partisi swap 2xRAM
> <br>3. Buat partisi untuk **/home** dengan ukuran paling maksimal
> <br>4. Buat juga partisi untuk **/var** dengan ukuran 30% dari partisi untuk **/home**

Baiklah, mari dieksekusi. Saat membuat catatan ini, saya menggunakan [VirtualBox](http://www.virtualbox.org) untuk menginstall Debian 9. Karena jika menggunakan **real server** tentunya saya tidak dapat mengambil tangkapan layar :-P. 

|![](/img/debian-9/install-debian-01.png)|
|Gambar 01: Tampilan awal|

Nyalakan VirtualBox, buat dan jalankan VM Debian sehingga tampil seperti Gambar 01 di atas. Ada beberapa pilihan di sana, saya akan menggunakan instalasi mode teks, maka saya ambil pilihan kedua, **Install**. Tekan tombol arah atas atau bawah untuk mengganti pilihan, lalu tekan **[ Enter ]**

|![](/img/debian-9/install-debian-02.png)|
|Gambar 02: Pilih Bahasa|

Cari dan pilih bahasa yang akan digunakan pada proses Instalasi, pilih **Bahasa Indonesia** untuk mengikuti catatan saya ini.

|![](/img/debian-9/install-debian-03.png)|
|Gambar 03: Yakin memilih bahasa Indonesia?|

Pilih **[ Ya ]** untuk melanjutkan dengan bahasa yang dipilih. Penjelasan pada gambar cukup jelas, saya kira.

|![](/img/debian-9/install-debian-04.png)|
|Gambar 04: Pilih lokasi|

Sekali lagi, pilih **Indonesia**.

|![](/img/debian-9/install-debian-05.png)|
|Gambar 05: Keyboard layout|

Untuk yang satu ini, biarkan default (**Inggris Amerika**), jangan diubah, langsung tekan **[ Enter ]**. Kecuali jika Anda yakin *keyboard layout* yang Anda gunakan bukan itu.

|![](/img/debian-9/install-debian-06.png)|
|Gambar 06: Mengatur jaringan|

Nah, pada konfigurasi jaringan ini saya memilih untuk tidak mengatur jaringan. NAT saya disable, jadi tidak akan dapat IP dari DHCP server. Pilih yang paling bawah, lalu tekan **[ Enter ]**.

|![](/img/debian-9/install-debian-07.png)|
|Gambar 07: Mengatur jaringan|

Atur nama host, nama host adalah nama komputer. Ini bebas, sesuka hati Anda. Dalam hal ini saya menggunakan *debian-server* sebagai hostname-nya. Tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-08.png)|
|Gambar 08: Kata sandi untuk akun **root**|

Ah, sebenarnya sudah cukup jelas penjelasan pada gambar tersebut. Mulai pada Debian 9 (Stretch) ini, akun super user **root** boleh tidak diberi kata sandi (password), sebagai gantinya sebuah akun pengguna akan dibuat dan diberi wewenang untuk menjadi root dengan perintah "**sudo**". Namun kebetulan, saya memilih untuk memasukkan kata sandi di sini :-P. Tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-09.png)|
|Gambar 09: Kata sandi lagi|

Masukkan kata sandi lagi.

|![](/img/debian-9/install-debian-10.png)|
|Gambar 10: Nama pengguna|

Ketikkan nama lengkap Anda, atau ketikkan **samsul maarif** jika Anda ingin menggunakan nama saya :-). Tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-11.png)|
|Gambar 11: Nama akun|

Nama akun biasanya otomatis akan menggunakan nama depan. Langsung tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-12.png)|
|Gambar 12: Kata sandi untuk akun pengguna|

Ketikkan kata sandi untuk akun pengguna tersebut. Kata sandi ini bersifat wajib. Ini digunakan untuk masuk ke sistem.

|![](/img/debian-9/install-debian-13.png)|
|Gambar 13: Zona waktu|

Pilih zona waktu yang sesuai dengan tempat tinggal Anda. Lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-14.png)|
|Gambar 14: Partisi hard disk|

Nah, ini nih. Inti dari catatan ini. Karena akan dilakukan instalasi dengan pemartisian manual. Pilih **Manual**, lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-15.png)|
|Gambar 15: Partisi hard disk|

Pada gambar 15 tersebut, terlihat partisi hardisknya masih kosong. 

> Jika pada hardisk Anda ada banyak partisi, pastikan data pada partisi hardisk tersebut sudah **DIBACKUP** alias **DICADANGKAN**, hapus semua partisi yang ada. Cara menghapusnya? Arahkan pilihan pada hardisk yang partisinya akan dihapus, tekan **[ Enter ]**, lalu pilih **yes** untuk menghapusnya. 
> <br>Jika Anda tidak cukup yakin, silakan pilih tombol **&lsaquo;kembali&rsaquo;** dan batalkan instalasi.

Setelah partisinya sudah kosong seperti gambar 15 tersebut, lanjutkan ke proses selanjutnya. Pilih pada **RUANG KOSONG**, lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-16.png)|
|Gambar 16: Buat partisi baru|

Pilih **Buat partisi baru**, lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-17.png)|
|Gambar 17: ukuran partisi|

Nah lagi, karena yang diminta **10 GB** untuk **/**, maka buat dulu partisinya. 

|![](/img/debian-9/install-debian-18.png)|
|Gambar 18: Jenis partisi|

Pilih **Primer**, lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-19.png)|
|Gambar 19: lokasi partisi|

Pilih **Awal**, lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-20.png)|
|Gambar 20: partisi **/**|

Jika berhasil akan tampil seperti Gambar 20 tersebut. Pastikan titik kait adalah **/**, sistem berkas yang saat ini saya gunakan adalah **sistem berkas berjurnal Ext4**. Pilih **Selesai menyusun partisi**, lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-21.png)|
|Gambar 21: buat partisi baru|

Selanjutnya, arahkan ke **RUANG KOSONG**, lalu tekan **[ Enter ]** untuk melanjutkan membuat partisi lagi.

|![](/img/debian-9/install-debian-22.png)|
|Gambar 22: buat partisi baru|

Pilih **Buat partisi baru**, lalu tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-23.png)|
|Gambar 23: partisi untuk swap|

Yang diminta pada soal adalah partisi swap 2xRAM, sedangkan RAM yang saya gunakan pada VirtualBox 1 GB. Jadi saya buat partisi baru berukuran **2 GB**, tekan **[ Enter ]** untuk melanjutkan

|![](/img/debian-9/install-debian-24.png)|
|Gambar 24: Jenis partisi baru|

Pilih **Logikal**, tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-25.png)|
|Gambar 25: lokasi partisi|

Penjelasannya di gambar sudah cukup jelas, kan? Pilih **Awal** saja, tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-26.png)|
|Gambar 26: ubah sistem berkas|

Nah, karena partisi ini akan digunakan sebagai swap, arahkan ke **Gunakan sebagai:**, tekan **[ Enter ]**.

|![](/img/debian-9/install-debian-27.png)|
|Gambar 27: pilihan sistem berkas|

Tarik ke bawah, dan pilih **ruang swap**, tekan **[ Enter ]** lagi.

|![](/img/debian-9/install-debian-28.png)|
|Gambar 28: swap selesai|

Tarik lagi ke bawah, pilih **Selesai menyususun partisi**, tekan **[ Enter ]** lagi.

|![](/img/debian-9/install-debian-29.png)|
|Gambar 29: ruang kosong|

Baru 2 partisi yang dibuat, selanjutnya arahkan lagi ke **RUANG KOSONG**, tekan **[ Enter ]** untuk membuat partisi baru.

|![](/img/debian-9/install-debian-30.png)|
|Gambar 30: buat partisi baru|

Sama seperti sebelumnya, pilih **Buat partisi baru**, tekan **[ Enter ]** lagi.

|![](/img/debian-9/install-debian-31.png)|
|Gambar 31: ukuran partisi|

Karena yang diminta adalah membuat partisi **/home** dan **/var**. Buat dulu partisi untuk **/var** agar lebih mudah, masukkan angka **30%**, tekan **[ Enter ]** untuk melanjutkan.

> Ini akan membuat partisi baru sebesar 30% dari sisa hardisk.

|![](/img/debian-9/install-debian-32.png)|
|Gambar 32: jenis partisi lagi|

Pilih **logikal** lagi, tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-33.png)|
|Gambar 33: lokasi partisi baru lagi|

Pilih **Awal**, tekan **[ Enter ]** untuk melanjutkan lagi.

|![](/img/debian-9/install-debian-34.png)|
|Gambar 34: ubah titik kait|

Arahkan pada **Titik kait** di mana di situ nampak **/home**, tekan **[ Enter ]**, akan muncul pilihan *titik kait*. 

|![](/img/debian-9/install-debian-35.png)|
|Gambar 35: pilihan titik kait|

Pilih **/var** untuk mengganti **/home**. Partisi untuk **/home** dibuat belakangan. Tekan **[ Enter ]** untuk melanjutkan.

|![](/img/debian-9/install-debian-36.png)|
|Gambar 36: Selesai menyusun partisi|

Setelah cukup yakin tampak seperti gambar di atas, bagian **Titik kait** diarahkan ke **/var**. Pilih **Selesai menyusun partisi**, tekan **[ Enter ]** di keyboard untuk melanjutkan.

|![](/img/debian-9/install-debian-37.png)|
|Gambar 37: Sisa ruang kosong|

Arahkan lagi ke **RUANG KOSONG**, **[ Enter ]** lagi di keyboard untuk melanjutkan.

|![](/img/debian-9/install-debian-38.png)|
|Gambar 38: Buat partisi baru|

Tekan **[ Enter ]** di keyboard untuk melanjutkan.

|![](/img/debian-9/install-debian-39.png)|
|Gambar 39: ukuran partisi baru|

Nah, ukuran partisi ini jangan diubah, karena berapa pun sisanya akan digunakan seluruhnya sebagai partisi untuk **/home**. Kecuali, jika akan membuat partisi lagi selain untuk yang sudah disebutkan di sini. Langsung tekan **[ Enter ]** di keyboard untuk melanjutkan tahap berikutnya.

|![](/img/debian-9/install-debian-40.png)|
|Gambar 40: Logikal|

Pilih jenis partisi **Logikal**, tekan **[ Enter ]** untuk melanjutkan lagi.

|![](/img/debian-9/install-debian-41.png)|
|Gambar 41: partisi **/home** |

Setelah tampak seperti gambar 41 di atas, **Titik kait** kali ini mengarah ke **/home**. Pilih **Selesai menyusun partisi**, **[ Enter ]** di keyboard untuk melanjutkan.

|![](/img/debian-9/install-debian-42.png)|
|Gambar 42: Selesai mempartisi|

Pada langkah ini, ditampilkan hasil pembuatan partisi yang tadi dibuat. Seperti pada gambar tersebut, terdapat sistem berkas, ukuran, titik kait, dll. 

> Catatan: 
> <br>- Partisi logikal selalu dimulai dengan angka #5, karena partisi primer maksimal adalah 4 partisi.
> <br>- **f** di situ bermakna partisi tersebut akan diformat. 

|![](/img/debian-9/install-debian-43.png)|
|Gambar 43: Konfirmasi pemartisian|

Sampai pada tahap ini, sebelum **&lsaquo;Ya&rsaquo;** dipilih, hardisk Anda masih tetap utuh. Belum diapa-apakan. Jadi, jika Anda membatalkan proses instalasi pada tahap ini, hardisk Anda masih utuh. 

Pada pertanyaan **Tulis perubahan yang terjadi pada hard disk?**, pilih **&lsaquo;Ya&rsaquo;**, tekan **[ Enter ]** untuk mengonfirmasi perubahan. Tunggu beberapa saat, hardisk Anda sedang diformat, dibuat beberapa partisi seperti yang diminta.

|![](/img/debian-9/install-debian-44.png)|
|Gambar 44: Repository dari DVD|

Saat muncul tampilan seperti pada gambar 44 di atas, pilih **&lsaquo;Tidak&rsaquo;**. 

> Seperti diketahui, Debian dibagi menjadi beberapa DVD, DVD 1 berupa installer dan berisi sebagian paket, DVD 2 & 3 berisi paket saja. Paket dalam hal ini dapat disebut sebagai repositori. Repositori merupakan gudang paket aplikasi. Saat menginstall aplikasi di Debian melalui paket manajemen, paket aplikasi tersebut diambil dari repositori. 
> 
> Memilih **&lsaquo;Ya&rsaquo;** akan mengharuskan Anda untuk memasukkan DVD 2 & 3, tentu akan memperpanjang proses instalasi.

|![](/img/debian-9/install-debian-45.png)|
|Gambar 45: gunakan mirror?|

Pilih **&lsaquo;Tidak&rsaquo;** juga, tekan **[ Enter ]** untuk mengonfirmasi. 

> Repository, baik daring atau melalui DVD dapat diatur setelah proses instalasi.

|![](/img/debian-9/install-debian-46.png)|
|Gambar 46: popularity contest|

Seperti tertulis pada gambar tersebut, hasil survey tersebut akan digunakan untuk menentukan paket-paket mana saja yang sebaiknya dimasukkan dalam CD Debian pertama. Pilih **&lsaquo;Ya&rsaquo;**, jika Anda berniat berpartisipasi, pilih **&lsaquo;Tidak&rsaquo;** jika Anda tidak berpartisipasi. Tekan **[ Enter ]** untuk mengonfirmasi.

|![](/img/debian-9/install-debian-47.png)|
|Gambar 47: Pilihan paket|

Karena akan menginstall Debian sebagai server, hanya beberapa paket yang akan diinstall. Gunakan tombol **[ Space ]** (spasi) di keyboard untuk mengaktifkan dan menon-aktifkan pilihan paket yang akan diinstall. 

|![](/img/debian-9/install-debian-48.png)|
|Gambar 48: SSH Server|

Hanya pilih **SSH Server** dan **Perkakas sistem standar** saja pada praktek ini. Tekan **[ Enter ]** untuk melanjutkan. Tunggu beberapa waktu hingga proses instalasi selesai, hingga muncul tampilan berikutnya.

|![](/img/debian-9/install-debian-49.png)|
|Gambar 49: Pasang boot loader|

Nah, cukup jelas penjelasan pada gambar tersebut, bukan? Pilih **&lsaquo;Ya&rsaquo;**, lalu tekan **[ Enter ]**.

|![](/img/debian-9/install-debian-50.png)|
|Gambar 50: Pilih hard disk|

Pilih perangkat (hard disk) untuk diinstall boot loader. Dalam hal ini hard disk satu-satunya adalah **/dev/sda**. Jika ada hard disk lain, pastikan pilih yang benar, yaitu hard disk yang dikaitkan dengan **/** tadi. Pilih **/dev/sda** dalam hal ini, lalu tekan **[ Enter ]**.

|![](/img/debian-9/install-debian-51.png)|
|Gambar 51: Instalasi selesai|

Sampai tahap ini instalasi telah selesai. Pilih **&lsaquo;Lanjutkan&rsaquo;** untuk mengakhiri instalasi dan komputer akan direstart. Untuk selanjutnya akan masuk ke sistem yang baru diinstall.

Pastikan CD/DVD media instalasi dikeluarkan. Sehingga saat komputer kembali dinyalakan tidak masuk lagi ke proses instalasi.

|![](/img/debian-9/install-debian-52.png)|
|Gambar 52: GRUB boot loader|

Tampilan GRUB Boot loader saat masuk ke sistem Debian yang baru diinstall.

|![](/img/debian-9/install-debian-53.png)|
|Gambar 53: tampilan login|

Ini adalah tampilan login, ibarat pintu masuk ke rumah yang baru dibuat. Masukkan nama pengguna dan kata sandi untuk masuk ke sistem.

> **CATATAN!** Kata sandi (password) di Linux tidak akan terlihat, jadi ketikkan saja kata sandi-nya, lalu tekan **[ Enter ]**

|![](/img/debian-9/install-debian-54.png)|
|Gambar 54: berhasil login|

Jika nama pengguna dan kata sandi yang diketikkan benar, Anda akan masuk ke sistem. Ditandai dengan **namapengguna@namahost:~$**, dalam hal ini nama pengguna saya adalah **samsul** dan nama host adalah **debian-server**.

> **Ups**, Anda lupa kata sandi yang diketikkan pada saat instalasi? Ya sudah, silakan install ulang sistem Anda. Pastikan tidak lupa lagi, itung-itung belajar memahami proses instalasi. Atau pelajari cara melakukan reset kata sandi untuk login ke sistem Linux, khususnya Debian.
