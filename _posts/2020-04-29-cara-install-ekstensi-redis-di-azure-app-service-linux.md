---
layout: post
title: Cara Install extensi redis di Azure App Service Linux
date: '2020-04-29'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Azure
cover-img:
  - "/img/path.jpg" : ""
share-img: "/img/path.jpg"
---

Pada proyek di salah satu klien di tempat saya bekerja, menggunakan layanan Azure app service untuk mendeploy aplikasi Laravel. Aplikasi tersebut membutuhkan ekstensi redis untuk dapat terhubung dengan cache service di azure yang berbasis Redis. Berikut langkah-langkah pemasangannya.

- Buka konsol KUDU https://\<sitename\>.scm.azurewebsites.net
- Pilih SSH
- pada konsol SSH, ketikkan perintah berikut

```
pear config-show
```

- perhatikan hasil keluaran perintah tersebut, catat nilai untuk `ext_dir` yang merupakan lokasi di mana ekstensi akan diinstal
- untuk menginstall ekstensi redis, gunakan perintah berikut:

```
root@5e1ea6aa20dc:/home/site/wwwroot# pecl install redis
.........
...........
Build process completed successfully
Installing '/usr/local/lib/php/extensions/no-debug-non-zts-20151012/redis.so'
install ok: channel://pecl.php.net/redis-4.1.1
configuration option "php_ini" is not set to php.ini location
You should add "extension=redis.so" to php.ini
```

- setelah ekstensi terpasang, seperti terlihat di atas lokasinya samna dengan nilai pada `ext_dir` sebelumnya. Salin berkas `redis.so` tersebut ke dalam direktori `/home/site/ext`

```
mkdir /home/site/ext
cp /usr/local/lib/php/extensions/no-debug-non-zts-20151012/redis.so /home/site/ext
```

- buat agar ekstensi tersebut dapat dimuat oleh sistem

```
mkdir /home/site/ini
echo "extension=/home/site/ext/redis.so" /home/site/ini/extensions.ini
```

- Selanjutnya, tambahkan application setting, buka Azure Portal (https://portal.azure.com), pilih aplikasi App Service Linux PHP `nama-app-service`-> **Settings** -> **Configuration** -> pada **Application Settings** klik pada **+ New application setting**
- masukkan nama dengan **PHP_INI_SCAN_DIR**
- masukkan nilainya dengan **/usr/local/etc/php/conf.d:/home/site/ini**
- simpan, lalu tadaaaaa.... gambar berikut menunjukkan ekstensi redis yang telah terpasang

![Ekstensi redis telah terpasang](https://i.imgur.com/qtHhvTM.png)

- Selesai.
- Oiya, langkah-langkah tersebut juga mungkin dapat dilakukan untuk memasang ekstensi lain.
- Semoga bermanfaat ya.
