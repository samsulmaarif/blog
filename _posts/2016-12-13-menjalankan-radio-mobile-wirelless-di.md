---
layout: post
title: Menjalankan Radio Mobile Wirelless di Ubuntu 16.04
date: '2016-12-13T15:20:00.000+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Onno W Purbo
- Linux
- Tutorial
- Ubuntu
- Workshop
- aplikasi
- Wireless
modified_time: '2016-12-13T15:20:21.152+07:00'
thumbnail: https://2.bp.blogspot.com/-0E_eIOUWgo0/WEaHyH_84PI/AAAAAAAANwM/1WpK7bkVLDc1o1fFSUrsuPROKEZldbQpgCPcB/s72-c/setelah-istirahat-full-day-oprek-radio-wireless-brebes.jpg
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-7137953649146992353
blogger_orig_url: http://blog.samsul.web.id/2016/12/menjalankan-radio-mobile-wirelless-di.html
---

[![](https://2.bp.blogspot.com/-0E_eIOUWgo0/WEaHyH_84PI/AAAAAAAANwM/1WpK7bkVLDc1o1fFSUrsuPROKEZldbQpgCPcB/s320/setelah-istirahat-full-day-oprek-radio-wireless-brebes.jpg)](https://2.bp.blogspot.com/-0E_eIOUWgo0/WEaHyH_84PI/AAAAAAAANwM/1WpK7bkVLDc1o1fFSUrsuPROKEZldbQpgCPcB/s1600/setelah-istirahat-full-day-oprek-radio-wireless-brebes.jpg)

Mengikuti [workshopnya](http://blog.samsul.web.id/2016/12/mengikuti-workshop-full-day-oprek-radio.html) pak Onno beberapa waktu lalu tepatnya pada hari Senin, 5 Desember 2016 di SMK Wicaksana Al Hikmah 2, Sirampog, Brebes, saya dan perserta lainnya diberi file berupa software Radio Mobile Wireless dan Ebook.  

Seperti dikutip dari [catatannya pak Onno](http://lms.onnocenter.or.id/wiki/index.php/Radio_mobile) :  

Radio Mobile adalah tool yang bebas dan powerfull untuk merencanakan pola radiasi radio dan memprediksi kinerja sistem radio. Menggunakan data elevasi terrain tersedia secara bebas dapat menghasilkan peta virtual skala abu-abu, x-ray dan warna pelangi. Di samping itu, juga dapat menghasilkan tampilan 3-D dan stereoscopic serta animasi sambil dioperasikan. Gambar latar belakang dapat digabungkan dengan peta hasil scan, foto satelit dan peta MapQuest untuk menghasilkan plot prediksi yang akurat. Anda dapat memperoleh salinan Radio Mobile dari situs resmi:  

[http://www.cplus.org/rmw/english1.html](http://www.cplus.org/rmw/english1.html)  

Saat workshop tersebut, para peserta tak perlu mendownload softwarenya satu per satu. Pak Onno telah menyediakan berupa folder bernama RMW, ukurannya kurang lebih 2 GB. Di dalam folder tersebut telah tersedia data ketinggian tanah untuk seluruh wilayah Indonesia. Juga ada sebuah eBook setebal 400-an halaman yang berjudul "**Jaringan Wireless di Dunia Berkembang**", dapat Anda unduh dari [sini](http://wndw.net/pdf/wndw-id/wndw-id-ebook.pdf).  

Awalnya saya kecewa karena flashdisk yang saya setorkan tidak muat untuk dicopy file tersebut. Ya, kecewa pada diri sendiri. Sempat minta dicopy dari laptop peserta lain yang telah mendapatkan filenya terlebih dahulu, namun ternyata ada virusnya. Walhasil file tersebut tidak dapat dijalankan di laptop saya. Bukan hanya itu, saat disalin ukurannya menjadi 100-an GB, padahal flashdisk saya kan cuma 16GB. Lucunya tuh di sini.  

Akhirnya saya kembali meminta file RMW ke pembicara, flashdisk saya pun diformat supaya filenya dapat disalin.  

Sembari menunggu, saya sempatkan menginstall **wine** dengan koneksi yang disediakan panitia. Kenapa? Karena untuk menjalankan aplikasi berbasis windows, sedangkan saya menggunakan Ubuntu Studio 16.04 64 bit.  

```
sudo apt install wine
```

Setelah file disalin, tanpa saya membaca [panduan ini](http://lms.onnocenter.or.id/wiki/index.php/RMW:_Instalasi_Radio_Mobile), saya pun iseng menjalankan :  

```
cd ~/Unduhan/RMW/
wine rmweng.exe
```

Lalu saya mendapatkan pesan error kurang lebih seperti ini:  

```
err:module:import_dll Library MSVBVM60.DLL
```

Googling sebentar, saya pun menemukan [solusinya](https://ubuntuforums.org/showthread.php?t=2144183), dengan menjalankan :  

```
winetricks
```

tampilannya akan seperti di bawah ini, pilih "select the default wine prefix" :  

[![](https://2.bp.blogspot.com/-yma9gGBEAiE/WE-qoJoE3ZI/AAAAAAAAOJE/vJ8m2-llQbYW4ZokgTGAGQrXEtDxEiRZwCLcB/s400/Cuplikan%2BLayar_2016-12-13_14-57-59.png)](https://2.bp.blogspot.com/-yma9gGBEAiE/WE-qoJoE3ZI/AAAAAAAAOJE/vJ8m2-llQbYW4ZokgTGAGQrXEtDxEiRZwCLcB/s1600/Cuplikan%2BLayar_2016-12-13_14-57-59.png)

Klik Ok, lalu di dialog selanjutnya pilih "**Install a Windows DLL component**" :  

[![](https://3.bp.blogspot.com/-ZWTzNqP7uJI/WE-re3lSRTI/AAAAAAAAOJQ/ziqt1ZW8PhIomqkMlzVKi1qbJseolcyAwCLcB/s400/Cuplikan%2BLayar_2016-12-13_14-58-19.png)](https://3.bp.blogspot.com/-ZWTzNqP7uJI/WE-re3lSRTI/AAAAAAAAOJQ/ziqt1ZW8PhIomqkMlzVKi1qbJseolcyAwCLcB/s1600/Cuplikan%2BLayar_2016-12-13_14-58-19.png)

Klik Ok lagi, scroll ke bawah, centang pada package "**vb6run**" yang pada Title bertuliskan "**MS Visual Basic 6 runtime sp6**"  

[![](https://4.bp.blogspot.com/-l8IKaVcfHok/WE-sMSG41SI/AAAAAAAAOJU/8pyjD-ybZMkB1F2675yGSro43SVQEeTaACLcB/s400/Cuplikan%2BLayar_2016-12-13_14-58-47.png)](https://4.bp.blogspot.com/-l8IKaVcfHok/WE-sMSG41SI/AAAAAAAAOJU/8pyjD-ybZMkB1F2675yGSro43SVQEeTaACLcB/s1600/Cuplikan%2BLayar_2016-12-13_14-58-47.png)

Klik Ok lagi, tunggu beberapa saat. Sistem akan mengunduh file yang diperlukan, artinya proses ini memerlukan koneksi internet.  

Setelah proses unduh selesai dan tidak ada error, kembali saya jalankan :  

```
wine rmweng.exe
```

Tada...  

[![](https://4.bp.blogspot.com/-bZql3IV75qo/WE-tWLepqdI/AAAAAAAAOJk/o7neT1V5rR0HywBEZpJh8_4XJ_J-B4t3gCLcB/s320/Cuplikan%2BLayar_2016-12-13_15-11-30.png)](https://4.bp.blogspot.com/-bZql3IV75qo/WE-tWLepqdI/AAAAAAAAOJk/o7neT1V5rR0HywBEZpJh8_4XJ_J-B4t3gCLcB/s1600/Cuplikan%2BLayar_2016-12-13_15-11-30.png)

Aplikasi Radio Mobile pun dapat dijalankan...  

[![](https://1.bp.blogspot.com/-gt6jfAGml10/WE-uE-brCqI/AAAAAAAAOJo/KtplPthzN0YHikwwgm6Lsq_lx_DK_zX3gCLcB/s400/radio-mobile-jalan-via-wine.jpg)](https://1.bp.blogspot.com/-gt6jfAGml10/WE-uE-brCqI/AAAAAAAAOJo/KtplPthzN0YHikwwgm6Lsq_lx_DK_zX3gCLcB/s1600/radio-mobile-jalan-via-wine.jpg)

Selanjutnya tinggal dikonfigurasi -> [http://lms.onnocenter.or.id/wiki/index.php/RMW:_Konfigurasi_Awal_Radio_Mobile_Wireless](http://lms.onnocenter.or.id/wiki/index.php/RMW:_Konfigurasi_Awal_Radio_Mobile_Wireless)  

Sekian dulu catatan kali ini. Semoga bermanfaat.
