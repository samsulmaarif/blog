---
layout: post
title: Menyiapkan Repo Lokal Debian 7.7
date: '2015-01-20T13:35:00.000+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Debian
- Tutorial
- Wheezy
- Repository
modified_time: '2015-01-20T14:04:40.809+07:00'
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-4724599543521624244
blogger_orig_url: http://blog.samsul.web.id/2015/01/menyiapkan-repo-lokal-debian-7.html
---

UKK sudah di depan mata. Kebanyakan tutorial yang tersebar di internet mengacu pada penggunaan Debian 6 (atau yang lebih lama). Sedangkan saat ini Debian telah merilis versi yang paling baru adalah 7.8\. Sejujurnya tulisan ini tidak hendak menawarkan solusi instant untuk permasalahan yang ada di debian 7.7 (versi ini yang saya miliki lengkap beserta repositorinya; DVD 1-3). Namun hanya sebagai alternatif pilihan jika memang hendak menggunakan Debian 7 (Wheezy).

Sedikit perbedaan antara Debian 6 & 7 setelah saya amati; beberapa paket yang diperlukan untuk diinstall pada saat ujian (`squid` salah satunya) tidak lagi tersedia di DVD-1 yang umumnya berupa DVD Installer. Ini cukup merepotkan jika kita hanya dapat menyediakan 1 DVD. Nah, kalaupun kita bisa menyediakan 3 DVD pada saat ujian, permasalahan selanjutnya adalah setelah beberapa kali dicoba untuk DVD-2 dan seterusnya tidak terdeteksi, hanya DVD-1 yang terdeteksi (_setidaknya itu yang saya alami_).

Untuk hal ini saya akan mencoba membuat sebuah repositori lokal. Secara teknis, ada beberapa cara alternatif untuk membuat sebuah repositori lokal;

1.  menggabungkan 3 DVD repo menjadi satu. Ini akan memudahkan pada saat instalasi, karena hanya perlu menuliskan satu baris sumber repositori pada berkas `/etc/apt/sources.list` pada sisi klien[1)](http://wiki.samsul.web.id/linux/Menyiapkan.Repo.Lokal.Debian.7#fn__1). Namun memerlukan waktu yang cukup lama untuk membuatnya, dari membongkar berkas iso, menggabungkannya, kemudian membuatkan berkas index untuk repositorinya.

2.  dengan metode mounting berkas iso secara langsung, kemudian buat sebuah _symbolic link_ ke `DocumentRoot` web server agar langsung dapat diakses oleh klien. Saya akan menggunakan cara ini, karena selain lebih cepat proses pembuatannya juga untuk menghemat ruang_hardisk_. ![:-P](http://wiki.samsul.web.id/lib/images/smileys/icon_razz.gif)

Sekedar informasi, sistem operasi yang saya gunakan adalah _Ubuntu Studio 14.04 64bit_, `apache2` web server telah terinstall dan siap digunakan dengan `DocumentRoot` berada di `/var/www/html/`. Kartu jaringan yang digunakan adalah `wlan0` untuk dihubungkan dengan metode `bridge`pada instalasi Debian 7 di _VirtualBox_. Menggunakan alamat jaringan 192.168.7.0/24 dengan DHCP telah diaktifkan.

```
[samsul@studio debian]$ pwd  
/media/samsul/Expansion Drive/ISO/debian  
[samsul@studio debian]$ ls -l | awk '{print $9}'  

debian-6.0.4-i386-DVD-1.iso  
debian-6.0.4-i386-DVD-1.list.gz  
debian-7.7.0-i386-DVD-1.iso  
debian-7.7.0-i386-DVD-2.iso  
debian-7.7.0-i386-DVD-3.iso  
debian-live-6.0.3-i386-standard.iso
```

Jika dilihat di situ, saya telah memiliki berkas iso debian 7.7 DVD 1 hingga 3\. Langkah selanjutnya adalah membuat direktori untuk mounting berkas iso tersebut:

```
[samsul@studio debian]$ sudo mkdir -p /media/debian/dvd{1,2,3}
```

Perintah tersebut akan membuat direktori `dvd1`, `dvd2`, dan `dvd3` pada `/media/debian/`. Kemudian mount berkas iso pada direktori yang sesuai:

```
[samsul@studio debian]$ sudo mount -t iso9660 -o loop debian-7.7.0-i386-DVD-1.iso /media/debian/dvd1
```

Lakukan hal yang sama untuk dvd2 dan dvd3\. Kemudian buat _symbolic link_ direktori `/media/debian/` ke `DocumentRoot` web server:

```
[samsul@studio debian]$ sudo ln -s /media/debian /var/www/html/debian
```

Uji hasilnya dengan mengakses alamat `[http://192.168.7.1/debian](http://192.168.7.1/debian "http://192.168.7.1/debian")` melalui browser:

![](http://wiki.samsul.web.id/lib/exe/fetch.php?w=600&tok=d0c132&media=http%3A%2F%2F3.bp.blogspot.com%2F-u5Cphl1j6L8%2FVL3x-FJkHfI%2FAAAAAAAADMk%2F1cYfKCAG23o%2Fs1600%2Frepo-debian-7-wheezy-browser.png)

Pastikan debian di virtualbox telah mendapatkan alamat IP yang sesuai:

![](http://wiki.samsul.web.id/lib/exe/fetch.php?w=600&tok=d4ed29&media=http%3A%2F%2F3.bp.blogspot.com%2F-TakzuWDMg2E%2FVL3zJ7JJrPI%2FAAAAAAAADM4%2FW_lcsS6ElQg%2Fs1600%2Fifconfig-debian-7-wheezy.png)

Sesuaikan isi berkas `/etc/apt/sources.list` seperti gambar berikut :

![](http://wiki.samsul.web.id/lib/exe/fetch.php?w=600&tok=93862a&media=http%3A%2F%2F3.bp.blogspot.com%2F-1RRLLk9kVY8%2FVL3x6ygmrzI%2FAAAAAAAADMc%2FVdeJejIsmBE%2Fs1600%2Fisi-berkas-sourceslist-debian-7-wheezy.png)

Pastikan baris lain telah ditutup (deberi tanda `#` di awal baris). Selanjutnya jalankan `aptitude update` pada debian.

![](http://wiki.samsul.web.id/lib/exe/fetch.php?w=600&tok=fb9796&media=http%3A%2F%2F4.bp.blogspot.com%2F-AFEkCzVZ350%2FVL3yAkM0h5I%2FAAAAAAAADMs%2FoigjFkE48_M%2Fs1600%2Fupdate-repo-debian7-wheezy.png)

Sampai di sini, konfigurasi repo Debian 7 telah berhasil. Kemudian coba install beberapa paket, kali ini saya coba install paket `squid3`.

![](http://wiki.samsul.web.id/lib/exe/fetch.php?w=600&tok=6a9a48&media=http%3A%2F%2F4.bp.blogspot.com%2F-4K_htOB_tAU%2FVL3x5xP7VDI%2FAAAAAAAADMU%2FOPB0jzuORbs%2Fs400%2Finstall-squid3-debian-7-wheezy.png)

Selesai sudah catatan ini, sebagai sebuah dokumentasi yang semoga ada manfaatnya. Catatan asli dari postingan ini [berasal dari sini](http://wiki.samsul.web.id/linux/Menyiapkan.Repo.Lokal.Debian.7).
