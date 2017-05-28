---
layout: post
title: Membuat Aplikasi Android dengan PhoneGap (Tanpa IDE)
date: '2015-01-05T01:33:00.000+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Tutorial
- nodejs
- phonegap
- aplikasi
- GitHub
- android
- cordova
- Ubuntustudio
modified_time: '2015-01-05T01:33:27.099+07:00'
thumbnail: http://3.bp.blogspot.com/-6GYj1PB0aq0/VKlyrl6-zFI/AAAAAAAADJo/-GEZ-ZEBjho/s72-c/config-xml-phonegap2.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-5762302384561496839
blogger_orig_url: http://blog.samsul.web.id/2015/01/membuat-aplikasi-android.html
---

Tidak pernah kubayangkan sebelumnya aku akan membuat sebuah aplikasi (untuk) Android. Tidak pernah kupikirkan pula apakah itu sulit atau mudah. Pada awalnya aku melihat, untuk membuat sebuah aplikasi Android, seseorang harus menguasai bahasa pemrograman Java. Sedangkan bahasa tersebut sampai saat ini belum pernah kupelajari.  

Ini hanya soal selera, aku bisa bilang aku tidak suka (bahasa pemrograman) Java (atau aku memang malas belajar). Tentu tak dapat dipungkiri, bahasa tersebut memang bahasa yang canggih, multiplatform, dan masih banyak lagi kelebihan dan keuntungan menggunakan bahasa tersebut.  

Sempat terpikir olehku, aku tidak akan pernah bisa membuat aplikasi Android. Sampai seorang teman mengenalkanku dengan **PhoneGap**. Sebenarnya, itu adalah kata kunci yang kudapatkan dari perbincangann dengannnya (atau kalau boleh kukatakan: _mendengarkan ceramahnya_ karena dia lebih banyak berbicara ketimbang aku :-P ). Dengan PhoneGap, aplikasi Android dapat dibuat tanpa harus kita menguasai Java, PhoneGap ini (dan banyak framework lainnya) menggunak teknologi HTML5, CSS, dan JavaScript.  

Sejujurnya, tulisan ini lebih kepada dokumentari (catatan pribadi), bukan tutorial.  

Oke, selanjutnya biar kutuliskan terlebih dahulu paltform apa yang kugunakan dan apa saja yang diperlukan sebelum dimulai masuk ke phonegap. Aku menggunakan UbuntuStudio 14.04 64 bit, tentunya beberapa perintah akan diketikkan dalam terminal di Linux.  

Yang dibutuhkan :  

*   [Node.js](https://www.npmjs.com/) : diperlukan untuk menginstall phonegap cli
*   Akun [GitHub](https://github.com/) : untuk menyimpan file-file project
*   [Git](http://git-scm.com/) : tentunya sedikit pemahaman tentang Git diperlukan untuk mengirim berkas project ke GitHub
*   Akun [Adobe ID di build.phonegap.com](https://build.phonegap.com/) : untuk membuat file APK secara online
*   Teks editor, geany, gedit, atau yang berbasis teks, VIM, nano. Dalam tutorial ini aku menggunakan geany.

Sebelum memulai, kita perlu menginstall Node.js dan Git terlebih dahulu, buka terminal lalu ketikkan perintah :  

```
sudo aptitude install npm git-core
```  

> **npm** adalah manajer paket untuk node.js

Selanjutnya, install phonegap :  

```
sudo npm install -g phonegap
```

Setelah semua terinstall, buat sebuah project baru dengan phonegap. Caranya adalah sebagai berikut :  

```
phonegap create nama-aplikasi
[phonegap] create called with the options /home/samsul/Project/nama-aplikasi com.phonegap.helloworld HelloWorld
[phonegap] Customizing default config.xml file
[phonegap] created project at /home/samsul/Project/nama-aplikasi
```

Pindah ke dalam folder aplikasi tadi  

```
cd nama-aplikasi
```

Nah, di dalam folder project tersebut akan berisi berkas-berkas sebagai berikut :  

> config.xml  <~~ berkas konfigurasi aplikasi,  
> .cordova  
> hooks  
> platforms  
> plugins  
> www       <~~ kode-kode aplikasi terletak di folder ini, ini yang kita "oprek"  

Di dalam file config.xml, ubah properti widget id, name, description, dan author, dan seterusnya, termasuk plugin yang akan digunakan :  


[![](http://3.bp.blogspot.com/-6GYj1PB0aq0/VKlyrl6-zFI/AAAAAAAADJo/-GEZ-ZEBjho/s1600/config-xml-phonegap2.png)](http://3.bp.blogspot.com/-6GYj1PB0aq0/VKlyrl6-zFI/AAAAAAAADJo/-GEZ-ZEBjho/s1600/config-xml-phonegap2.png)

Klik untuk melihat [config.xml](https://github.com/samsulmaarif/berita-muktisari/blob/master/www/config.xml) Desa Muktisari

Langkah selanjutnya adalah mengirim file project ke GitHub. Git perlu dikonfigurasi sebelum dapat digunakan, [klik tautan berikut](https://help.github.com/articles/set-up-git/) untuk tutorialnya. Setelah itu, ketikkan perintah berikut (perintah ini hanya perlu diketikan sekali saja) :  

```
git init
Initialized empty Git repository in /home/samsul/Project/nama-aplikasi/.git/
```

[Buat sebuah repsitori di GitHub](https://help.github.com/articles/create-a-repo/), lalu ikuti petunjuknya untuk repositori lokal kita, kurang lebih caranya sebagai berikut :  

```
git remote add origin https://github.com/username/nama-repositori.git
```

Perintah-perintah berikut adalah untuk mengirim perubahan ke GitHub,  

```
git add -A
git commit -m "Commit pertama"  <~~ sesuaikan commit message, sesuai perubahan yang dikirim  
git push -u origin master
```

Jika kita sudah selesai oprekisasi (_nge-viky_), kita dapat langsung mencoba di ponsel Android dengan bantuan sebuah [aplikasi](http://app.phonegap.com/) (aku sendiri belum coba pake ini, karena tidak mendukung di ponselku). Atau langsung dibuat APK dengan PhoneGap Build.  

Sebenarnya ada 2 cara untuk membuat APK dengan PhoneGap, yaitu secara lokal dan remote (via web build.phonegap.com). Kalau ingin secara lokal, Android SDK harus sudah terinstall di komputer/laptop kita. Asumsikan Android SDK sudah terinstall, maka kita dapat mengetikkan perintah berikut untuk membuat APK secara lokal :  

```
phonegap local build android
```

Nah, untuk membuat APK secara online, buka [https://build.phonegap.com/apps](https://build.phonegap.com/apps) di browser dan login dengan akun kita. Klik tombol "**new app**", lalu pastekan link repositori github kita tadi (https://github.com/username/nama-repositori.git) , kemudian klik "**Pull from .git repository**", aplikasi akan di-build secara otomatis.  

[![](http://4.bp.blogspot.com/-SpBVL1VtNrY/VKmCoCQJCcI/AAAAAAAADJ4/7IFl1dqp1q4/s1600/build-phonegap-com-desa-muktisari.png)](http://4.bp.blogspot.com/-SpBVL1VtNrY/VKmCoCQJCcI/AAAAAAAADJ4/7IFl1dqp1q4/s1600/build-phonegap-com-desa-muktisari.png)

Pastikan tidak terjadi kesalahan (error) dalam proses ini, perbaiki jika ada, lalu push kembali ke repositori GitHub. Langkah selanjutnya akan mudah, klik _update code_, lalu pull latest, aplikasi akan dibangun kembali. APK siap diunduh dan ditest di ponsel (smartphone) kita. Jika berhasil, file APK akan bernama _NamaAplikasi-debug.apk_

Demikian, semoga bermanfaat dan tidak membingungkan.
