---
layout: post
title: Cara Praktis Menghapus Ribuan Komentar Spam WordPress
date: '2017-09-27'
author: Samsul Maarif
categories: blog
tags:
  - Wordpress
  - MySQL
  - Spam
  - Linux
  - Komentar
bigimg: 
  - "/img/taman-singha-merjosari-malang-2018.jpg" : "Taman Singha Merjosari, Malang (2018)"
share-img: "/img/taman-singha-merjosari-malang-2018.jpg"
---

Saat melakukan maintenance di website salah satu klien kami, ada saja website yang kena spam. Banyak sekali komentar yang masuk, namun komentar tersebut nampaknya dibuat oleh bot. Mungkin admin websitenya lupa menyalakan Akismet untuk menangkal komentar spam.

Kalau jumlahnya hanya puluhan, mungkin dapat dengan mudah dihapus dengan sebuah tombol di halaman dashboard WordPress. Bagaimana jika komentarnya mencapai ribuan, atau ratusan ribu, atau bahkan jutaan komentar?

## Hapus dengan Perintah SQL

Anda dapat menggunakan perintah SQL untuk menghapus ribuan komentar spam di WordPress. WordPress memiliki tabel yang bernama wp_comments. Tabel tersebut digunakan untuk menyimpan, mengambil, memodifikasi, dan menghapus komentar.

Pertama, login ke server melalui ssh. Selanjutnya, ketikkan perintah berikut untuk masuk ke mysql database.

> Perhatikan! Sebelum menjalankan perintah berikut, sangat disarankan untuk melakukan backup database Anda. Jika Anda menemui masalah setelah menjalankan perintah ini, Anda dapat dengan mudah melakukan restore.

`mysql -u nama-pengguna-database -p nama-database`

atau

`mysql -u nama-pengguna-database -h hostname-atau-ip-database-server -p nama-database`

Pada contoh berikut, saya menggunakan nama pengguna dan nama database mywordpress.

`mysql -u mywordpress -p mywordpress`

atau

`mysql -u mywordpress -h localhost -p mywordpress`

Ketikkan passwordnya, lalu tekan Enter.

> Jika tidak memiliki akses ssh, perintah berikut juga dapat dijalankan melalui phpMyAdmin.

Setelah berhasil masuk, ketikkan describe wp_comments; untuk melihat struktur tabel wp_comments.

`MariaDB [mywordpress]> describe wp_comments;`

Untuk melihat semua komentar yang belum dikonfirmasi, ketikkan perintah berikut :

`MariaDB [mywordpress]> select * from wp_comments where comment_approved = '0';`

Atau jika terlalu banyak, kita juga dapat memodifikasi perintah tersebut untuk mengetahui jumlahnya :

`MariaDB [mywordpress]> select count(*) from wp_comments where comment_approved = '0';`

Contoh outputnya :

```
+----------+
| count(*) |
+----------+
|     1721 |
+----------+
1 row in set (0.33 sec)
```

Sedangkan untuk melihat komentar yang statusnya sudah menjadi spam, ubah ‘0’ menjadi ‘spam’ :

`MariaDB [mywordpress]> select * from wp_comments where comment_approved = 'spam'`

## Perintah SQL untuk menghapus komentar spam

Nah, jika sudah cukup yakin, jalankan perintah berikut untuk menghapus semua komentar yang belum dikonfirmasi :

`MariaDB [mywordpress]> delete from wp_comments where comment_approved = '0';`

Contoh outputnya :

`Query OK, 1721 rows affected (0.25 sec)`

Hapus juga komentar yang sudah berstatus sebagai spam :

`MariaDB [mywordpress]> delete from wp_comments where comment_approved = 'spam';`

Ini belum berakhir, untuk menghemat ruang di database, kita juga dapat menghapus metadata komentar. Metadata komentar ini yang berhubungan dengan komentar yang ada di WordPress.

`MariaDB [mywordpress]> delete from wp_commentmeta where meta_key like "%akismet%";`

Perintah ini akan menghapus setiap metadata komentar yang pernah disentuh oleh Akismet. Plugin WordPress untuk menangkal komentar spam.

> Catatan: Copas dari tulisan lama di blog sebelah, tapi tulisan saya sendiri lho....