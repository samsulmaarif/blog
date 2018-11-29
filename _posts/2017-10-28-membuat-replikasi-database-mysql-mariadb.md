---
layout: post
title: Membuat Replikasi Database MySQL/MariaDB
date: '2017-10-28'
author: Samsul Maarif
categories: blog
tags:
- Replikasi
- MySQL
- MariaDB
- Linux
bigimg: 
  - "/img/kampung-3d-jodipan-malang-2018-11.jpg" : "Kampung 3D Jodipan, Malang (2018)"
share-img: "/img/kampung-3d-jodipan-malang-2018-11.jpg"
---

## Tentang Replikasi Database MariaDB

Replikasi Database MariaDB adalah sebuah proses yang memungkinkan administrator untuk mengelola beberapa salinan data MariaDB agar terduplikasi secara otomatis dari master ke database slave. Ini dapat bermanfaat untuk berbagai hal, termasuk pencadangan data (baca: backup), menganalisa data tanpa menggunakan basisdata utama, atau untuk keperluan scale out (mengarah ke BIG DATA -pen).

Catatan ini akan mencakup contoh kecil replikasi database dengan MariaDB, satu master akan mengirim informasi ke slave tunggal. Untuk proses kerjanya, memerlukan 2 alamat IP, satu server master dan lainnya slave.

Dalam hal ini, saya akan menggunakan alamat IP sebagai berikut :

- 10.19.19.251 – sebagai Database Master
- 10.19.19.74 – sebagai Database Slave

## Persiapan

Catatan ini mengasumsikan Anda telah memiliki 2 buah VM dan memiliki akses sudo terhadap 2 VM tersebut. Sistem Operasi yang akan saya gunakan adalah Ubuntu 16.04 server, MariaDB telah terinstall dan belum dikonfigurasi. Anda dapat menggunakan berbagai teknologi untuk membuat VM, baik menggunakan VirtualBox, Libvirt, Proxmox VE, atau bahkan VMware.

Jika MariaDB belum terinstall, Anda dapat menginstallnya dengan perintah ini :

```
$ sudo apt install mariadb-server mariadb-client
```

### Langkah 1: Atur Konfirugasi Database Master

Buka konfigurasi MariaDB pada server master:

```
$ sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Setelah masuk dalam berkas tersebut, ubah beberapa parameter berikut :

Pertama cari baris seperti berikut,

```
bind-address                   = 127.0.0.1
```

Ganti alamat IP bawaan tersebut dengan alamat IP servernya, dalam hal ini adalah 10.19.19.251:

```
bind-address                   = 10.19.19.251
```

Selanjutnya konfigurasikan server-id. Konfigurasi ini terletak pada bagian [mysqld]. Anda dapat memilih angka berapa saja untuk konfigurasi ini, namun angkanya harus unik dan tidak boleh ada yang sama antara kelompok replikasi. Untuk database master ini cukup gunakan 1 saja. Pastikan baris berikut tidak diberi komentar (tanda # di awal baris).

```
server-id                      = 1
```

Selanjutnya baris log_bin. Semua rincian informasi replikasi akan disimpan di sini. Database slave akan menyalin semua perubahan yang terdaftar dalam log ini. Untuk langkah ini, cukup hapus tanda # di depan log_bin.

```
log_bin                        = /var/log/mysql/mysql-bin.log
```

Anda juga perlu mendefinisikan database apa yang akan direplikasi di server slave. Anda dapat menyertakan lebih dari satu database dengan mengulangi baris ini untuk setiap database yang diperlukan.

```
binlog_to_db                   = myappdb
```

Simpan berkas konfigurasi tersebut dengan menekan Ctrl+X, lalu Y, dan Enter. Restart MariaDB.

```
$ sudo systemctl restart mysql
```

Langkah berikutnya, masuk ke shell MariaDB.

```
$ sudo -i
# mysql -u root -p
```

Buat database yang akan direplikasi, lewati langkah ini jika sudah ada databasenya.

```
MariaDB [(none)]> CREATE DATABASE myappdb;
```

Beri akses slave dengan mengetikkan perintah berikut pada shell MariaDB.

```
MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%' IDENTIFIED BY 'rahasia';
```

Nama pengguna yang digunakan adalah slave_user dan passwordnya adalah rahasia. Nama pengguna dan sandi tersebut akan digunakan oleh slave untuk mengakses dan mereplikasi database dari master. Ganti sesuai yang Anda inginkan. Lalu,

```
MariaDB [(none)]> FLUSH PRIVILEGES;
```

Buka tab/jendela baru untuk melakukan langkah berikutnya. Pada tab baru tersebut, masuk kembali ke MariaDB dan gunakan database yang akan direplikasi.

```
MariaDB [(none)]> USE myappdb;
```

Jalankan perintah berikut untuk mencegah (baca: mengunci) perubahan pada database tersebut.

```
MariaDB [myappdb]> FLUSH TABLES WITH READ LOCK;
```

Lalu ketikkan :

```
MariaDB [myappdb]> SHOW MASTER STATUS;
```

Jika berhasil, akan muncul tabel kira-kira sebagai berikut:

```
MariaDB [myappdb]> SHOW MASTER STATUS;                            
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000001 |      312 | myappdb     |                  |
+------------------+----------+--------------+------------------+
1 row in set (0.00 sec)
```

Ini posisi di mana database slave akan mulai mereplikasi. Catat nomor ini, karena akan digunakan pada langkah berikutnya. Jika Anda melakukan perubahan pada jendela yang sama, database tersebut akan secara otomatis terbuka kuncinya.

Untuk alasan inilah, Anda perlu membuka tab/jendela baru untuk melanjutkan ke langkah berikutnya.

Saat database sedang terkunci, ekspor database menggunakan mysqldump pada jendela baru. (pastikan perintah ini dijalankan pada shell bash, bukan MariaDB).

```
# mysqldump -u root -p --opt myappdb > myappdb.sql
```

Sekarang, kembali ke jendela sebelumnya, buka kunci database (buat agar dapat diubah kontennya).

```
MariaDB [myappdb]> UNLOCK TABLES;
```

Keluar dari shell MariaDB dengan mengetikkan exit; atau \q

```
MariaDB [myappdb]> \q
Bye
```

Konfigurasi pada master telah selesai.

### Langkah 2: Konfigurasi pada Database Slave

Setelah konfigurasi database master selesai, sekarang konfigurasikan database slave.

Masuk ke server slave, buka shell MariaDB dan buat database baru dengan nama yang sama, lalu keluar.

```
MariaDB [(none)]> CREATE DATABASE myappdb;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> \q
Bye
```

Import database yang sebelumnya diekspor dari database master.

```
# mysql -u root -p myappdb < /lokasi/file/myappdb.sql
```

Selanjutnya konfigurasikan seperti yang dilakukan pada master.

```
$ sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Pastikan beberapa konfigurasi berikut diatur dengan tepat. Pertama adalah server-id. Seperti yang disebutkan sebelumnya, nomor server-id di sini harus unik. Tidak boleh ada yang sama antara satu kelompok master-slave. Karena bawaannya adalah 1, pastikan ubah dengan angka yang berbeda.

```
server-id                      = 2
```

Pastikan setidaknya kriteria konfigurasi berikut terisi dengan tepat:

```
relay-log                      = /var/log/mysql/mysql-relay-bin.log
log_bin                        = /var/log/mysql/mysql-bin.log
binlog_do_db                   = myappdb
```

Anda perlu menambahkan baris relay-log secara manual. Baris tersebut tidak ada sebelumnya. Setelah perubahan konfigurasi telah dibuat sebagaimana diinginkan, simpan dan keluar dari berkas konfigurasi.

Restart MariaDB sekali lagi.

```
$ sudo systemctl restart mysql
```

Langkah berikutnya, aktifkan replikasi dengan shell MariaDB. Buka shell MariaDB sekali lagi dan ketikkan perintah berikut, ubah-suaikan dengan konfigurasi Anda:

```
MariaDB [(none)]> CHANGE MASTER TO
 -> MASTER_HOST='10.19.19.251',
 -> MASTER_USER='slave_user',
 -> MASTER_PASSWORD='rahasia',
 -> MASTER_LOG_FILE='mysql-bin.000001',
 -> MASTER_USE_GTID = current_pos,
 -> MASTER_LOG_POS= 312;
Query OK, 0 rows affected (0.30 sec)
```

Perintah tersebut menyelesaikan beberapa peperjaan sekaligus:
1). ini menunjuk server saat ini sebagai slave dari master server kita yang beralamat di 10.19.19.251,
2). menyediakan kredensial masuk yang tepat pada server,
3). memungkinkan slave tahu dari mana harus mulai melakukan replikasi; master log file dan log posisi berasal dari angka yang ditulis sebelumnya.

Dengan begini, Anda telah mengonfigurasi replikasi database master dan slave. Lalu aktifkan server slave:

```
MariaDB [(none)]> START SLAVE;
Query OK, 0 rows affected (0.00 sec)
```

Anda dapat melihat detil informasi replikasi slave dengan mengetikkan perintah ini. Pilihan \G di belakang perintah berikut untuk menata teks agar dapat lebih mudah dibaca.

```
MariaDB [(none)]> SHOW SLAVE STATUS\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.19.19.251
                  Master_User: slave_user
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 312
               Relay_Log_File: mysql-relay-bin.000002
                Relay_Log_Pos: 535
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table:
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 312
              Relay_Log_Space: 832
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 1
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: Current_Pos
                  Gtid_IO_Pos: 
1 row in set (0.00 sec)
```

Jika ada masalah koneksi, Anda dapat menjalankan slave dengan perintah skip:

```
MariaDB [(none)]> SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1; SLAVE START;
```

Selesai.
Cek Hasilnya

Pada database master, buatlah sebuah tabel, misal dengan perintah berikut :

```
MariaDB [myappdb]> CREATE TABLE myapp_user (
    -> id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    -> firstname VARCHAR(30) NOT NULL,
    -> lastname VARCHAR(30) NOT NULL,
    -> email VARCHAR(50),
    -> reg_date TIMESTAMP
    -> );
```

Lalu periksa pada database slave. Jika berhasil, sebuah tabel akan dibuat di sana, sama persis dengan yang di master:

```
MariaDB [myappdb]> SHOW TABLES;
+--------------------+
| Tables_in_myappdb |
+--------------------+
| myapp_user         |
+--------------------+
MariaDB [myappdb]> DESCRIBE myapp_user;

```

Berhasil.

> CATATAN:
> Mulai MariDB 10.0 dikenalkan fitur baru berupa global transaction ID (GTID) untuk replikasi. Umumnya direkomendasikan untuk menggunakan GTID dari MariaDB 10.0, karena memiliki berbagai kelebihan. Yang diperlukan hanyalah menambahkan pilihan MASTER_USE_GTID ke statement CHANGE MASTER, misalnya :

```
CHANGE MASTER TO MASTER_USE_GTID = current_pos
```

Selamat mencoba.

## Referensi :

- https://mariadb.com/kb/en/library/setting-up-replication/
- https://www.digitalocean.com/community/tutorials/how-to-set-up-master-slave-replication-in-mysql
