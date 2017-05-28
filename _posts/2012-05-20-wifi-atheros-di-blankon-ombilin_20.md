---
layout: post
title: Wifi Atheros di BlankOn Ombilin
date: '2012-05-20T06:02:00.002+07:00'
author: Samsul Maarif
categories: blog
tags: 
modified_time: '2012-05-20T11:42:00.676+07:00'
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-4445911564195376851
blogger_orig_url: http://blog.samsul.web.id/2012/05/wifi-atheros-di-blankon-ombilin_20.html
---

Seorang teman yang baru kukenal datang ke tempatku dengan membawa laptop kesayangannya Axioo Neon CNC. Ternyata dia adalah temannya teman kuliahku. Saat baru datang dia menanyakan apakah 'kehadiran' Akbar Bahaulloh di tempatku. Ketika kutanya keperluannya, ternyata dia memiliki masalah di laptopnya : "Windowsnya error!".

Pertanyaan pertama yang diajukannya adalah "Apa ada linux Axioo?" saya tidak yakin dengan jawaban saya, tapi saya hanya menunjukkan kolekdi CD/DVD linux yang saya punya. Kebetulan salah satunya yang belum lama saya bakar tersedia DVD [BankOn Sajadah Ombilin](http://sajadah.blankonlinux.or.id/ "BlankOn Sajadah").

Ketika laptop sudah dinyalakan, tidak ada tanda-tanda kehidupan sistem operasi windows yang telah terinstall. Yang ada hanyalah pesan untuk recovery, "Start windows normally" dan pesan-pesan error lain yang hanya dimengerti oleh teknisi Microsoft.

Ada yang unik dari laptop ini, seperti yang disampaikan oleh pemiliknya, dia pernah dengar dari penjual laptop ini bahwa laptop ini tidak dapat diinstali Windows bajakan. Benar saja, tak kurang dari lima kali bolak-balik-keluar-masuk servisan sini-servisan sana, teknisi sini-teknisi sana tidak ada yang berhasil menginstall windows bajakan ke laptop tersebut. Kecuali katanya ada seorang tetangganya yang memiliki CD/DVD Installer Windows yang asli. Beberapa kali laptop tersebut telah diinstall ulang oleh tetangganya tersebut.

Namun pada hari ini dia datang ke tempatku untuk menemui kawanku Akbar Bahaulloh yang juga akan datang ke tempatku. Karena tetangganya tersebut saat ini tidak sedang berada di posisinya. Lalu kami mencoba untuk menginstall W7 di dalamnya, tapi hasilnya selalu nihil, selalu macet dan hang. Secara iseng dan spontan (tentu saja) saya mencoba DVD BlankOn Sajadah Ombilin yang saya miliki di laptop tersebut.

And _violla_, almost everything works perfectly. Resolusi monitor maksimum, LAN terdeteksi normal, suara juga. Hanya satu masalahnya, yaitu Wireless Atheros AR9285 yang tidak terdeteksi secara langsung. Dan hal ini yang saya bahas pada tulisan ini.

Berbekal pengalaman sebelumnya ketika "ngoprek" (apa sih istilah yang lebih sederhana?) Atheros di [Ubuntu 10.10](http://www.ubuntu.com), yang saya lakukan kira-kira seperti sebagai berikut :

Edit file /etc/modprobe.d/blacklist-ath_pci.conf dengan teks editor favorit, misalnya

$ sudo vim  /etc/modprobe.d/blacklist-ath_pci.conf

tambahkan komentar pada sebelum "blacklist ath_pci"

# blacklist ath_pci

lalu simpan file tersebut. Setelah saya logout, lalu login kembali (jika memang diperlukan lakukan restart) Wifi Atheros AR9285 kini telah terdeteksi. Pastikan switch Wifi sudah di-on-kan. :D

Ya, Anda benar. Bawaan insalasi BlankOn Ombilin dan beberapa distro lainnya memang memblacklist chipset Atheros dan Broadcom, seperti yang disebutkan pada file /etc/modprobe.d/blacklist-ath_pci.conf :

# For some Atheros 5K RF MACs, the madwifi driver loads buts fails to  
# correctly initialize the hardware, leaving it in a state from  
# which ath5k cannot recover. To prevent this condition, stop  
# madwifi from loading by default. Use Jockey to select one driver  
# or the other. (Ubuntu: #315056, #323830)

Berikut ini lampiran tentang informasi hardware yang berhasil dihimpun : [cpuinfo](http://tempel.blankon.in/6528.txt "cpuinfo"), [lspci](http://tempel.blankon.in/6529.txt "lspci"), [lshw](http://tempel.blankon.in/6530.txt "lshw"), [dmesg](http://tempel.blankon.in/6531.txt "dmesg"), hwinfo, [lsusb](http://pastebin.com/AUq71XsG "lsusb"), [meminfo](http://tempel.blankon.in/6532.txt "meminfo").
