---
layout: post
title: Mengunci Versi aplikasi di CentOS
date: '2020-10-12'
author: Samsul Maarif
categories: blog
tags:
- Linux
- CentOS
cover-img:
  - "/img/sawah-sidareja.jpg" : "Purwosari, Kedungreja, Cilacap (2019)"
share-img: "/img/sawah-sidareja.jpg"
---

Setelah catatan sebelumnya kita berhasil [downgrade versi aplikasi di CentOS](/2020/09/cara-downgrade-aplikasi-centos.html), ternyata saat kita lakukan `yum update`, aplikasi kembali dinaikkan versinya ke versi yang lebih baru. Tentunya hal ini menjengkelkan karena aplikasi project kita akan kembali error alias tidak jalan.

Namun jangan khawatir, pada postingan ini kita akan membahas bagaimana cara mengunci versi aplikasi di CentOS.

## Install yum plugin lockversion

Install plugin yum lockversion berikut dengan cara berikut:

```
yum install yum-plugin-versionlock
```

file `/etc/yum/pluginconf.d/versionlock.list` akan secara otomatis dibuat dalam sistem.

Untuk mengunci paket yang telah terinstall dapat dilakukan dengan:

```
yum versionlock dotnet-*
yum versionlock aspnetcore-*
```

dengan demikian paket dotnet-* tidak akan diupgrade ke versi yang lebih baru. Menghindari issue yang terjadi saat deployment aplikasi karena perbedaan versi dotnet.

Untuk melihat daftar paket yang dikunci:

```
yum versionlock list
```

Kalo misalnya sudah **TIDAK** membutuhkan versionlock, bersihkan daftar tersebut dengan

```
yum versionlock clear
```

Demikian.

## Referensi
- [https://access.redhat.com/solutions/98873](https://access.redhat.com/solutions/98873)

{: .box-success }
Ingin belajar lebih jauh tentang server Linux & teknologi *Container*? atau ingin menyewa jasa kami? boleh lah intip ke sini --> [https://nacita.id](https://nacita.id/layanan/jasa/)
