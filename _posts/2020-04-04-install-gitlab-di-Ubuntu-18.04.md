---
layout: post
title: Install GitLab di Ubuntu 18.04
date: '2020-04-04'
author: Samsul Maarif
categories: blog
tags:
- Linux
- GitLab
- DevOps
cover-img:
  - "/img/tugu-malang-2019.jpg" : "Tugu Malang, Kota Malang (2019)"
share-img: "/img/tugu-malang-2019.jpg"
---

Setelah sekian lama, saya merasa perlu memerbarui konten blog ini. Kali ini saya akan menulis ulang panduan instalasi GitLab di Ubuntu 18.04 dalam Bahasa Indonesia. Dokumentasi resminya dapat langsung diakses melalui [halaman ini](https://about.gitlab.com/install/#ubuntu). Di sana cukup singkat dan mudah dipahami. Namun saya menambah beberapa cuil penjelasan tambahan berdasarkan hasil praktek yang saya lakukan di postingan ini.

**GitLab** merupakan sebuah aplikasi lengkap untuk pengembangan perangkat lunak (software development). Selain fitur utama sebagai manajer repositori Git berbasis web, ratusan fitur lainnya juga tak kalah menarik untuk diulik. Seperti tertulis di laman websitenya, GitLab merupakan platfom lengkap untuk siklus DevOps (Development and Operation).

Berbeda dengan rivalnya, yaitu GitHub, GitLab merupakan aplikasi yang bersifat *open source* alias sumber terbuka dan dapat kita install di server kita sendiri. Nah, GitLab yang dapat kita install sendiri ini ada dua edisi. Edisi Komunitas (Community Edition; CE) dan Edisi Perusahaan (Enterprise Edition; EE), pada praktek kali saya hanya akan menggunakan edisi komunitas.

Spesifikasi server (VM) yang saya gunakan sebagai berikut:

Komponen | Spesifikasi
--- | ---
vCPU | 2 core
Memory | 4 GB
Disk | 40 GB

Untuk membaca kebutuhan minimal lengkapnya silakan buka halaman [dokumentasinya di sini](https://docs.gitlab.com/ee/install/requirements.html#hardware-requirements).

Lakukan update dan install paket dasar yang dibutuhkan:

```
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates
```

Selanjutnya install postfix untuk mengirim notifikasi email. Boleh skip jika menggunakan solusi pengiriman lain, dan konfigurasi SMTP eksternal setelah GitLab selesai diinstall.

```
sudo apt-get install -y postfix
```

Saat proses instalasi, akan muncul layar konfigurasi. Pilih "Internet Site" dan tekan enter. Gunakan nama domain eksternal untuk 'mail name' dan tekan enter. Saat layar selanjutnya muncul, tekan enter untuk menerima nilai defaultnya.


Tambahkan repositori GitLab:

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```

Selanjutnya, install paket GitLab. Ganti `https://gitlab.nacita.id` dengan URL yang akan Anda gunakan. Instalasi akan secara otomatis mengonfigurasi dan menjalankan GitLab dengan URL tersebut.

Untuk URL `https://` GitLab akan meminta sertfikat dengan Let's Encrypt secara otomatis, langkah ini memerlukan akses masuk HTTP dan hostname yang benar. Anda dapat juga menggunakan sertifikat Anda sendiri atau gunakan saja `http://`.


```
sudo EXTERNAL_URL="https://gitlab.nacita.id" apt-get install gitlab-ce
```

Akses melalui browser ke alamat `https://gitlab.nacita.id` (URL yang Anda gunakan pada parameter EXTERNAL_URL di atas)

Anda mungkin perlu mengonfigurasi A record pada DNS manager Anda untuk dipointing pada alamat IP public server GitLab Anda.

![Buat root password](https://i.imgur.com/RtAfAFY.png)

Saat pertama kali dibuka, Anda akan diarahkan ke halaman reset kata sandi. Masukkan password untuk akun administrator pertama, dan Anda akan diarahkan kembali ke halaman login. Gunakan nama pengguna default `root` untuk masuk.

![Tampilan awal setelah login sebagai root](https://i.imgur.com/ocuD4hW.png)

Baca dokumentasi lengkapnya pada halaman [instalasi dan konfigurasi](https://docs.gitlab.com/omnibus/README.html#installation-and-configuration-using-omnibus-package).

Agar lebih jelas, saya buatkan sreencast proses instalasinya sebagai berikut :

[![asciicast](https://asciinema.org/a/lPp5LqT6yB3sQiokbM4JuED9y.svg)](https://asciinema.org/a/lPp5LqT6yB3sQiokbM4JuED9y)

# Konfigurasi GitLab

Setelah proses instalasi selesai, biasanya kita perlu melakukan beberapa perubahan dalam instance GitLab. Misalnya perubahan domain, aktifasi ssl, konfigurasi email, smtp, penggunaan eksternal database, dll. Nah, semua konfigurasi tersebut tersimpan dalam sebuah file bernama `gitlab.rb`, langsung aja kita edit sebagai berikut:

```
sudo vim /etc/gitlab/gitlab.rb
```

Beberapa konfigurasi yang saya lakukan antara lain:

```
# line 29 nama domain valid yang akan digunakan
external_url 'https://gitlab.nacita.id'

# line 64 ubah zona waktu
gitlab_rails['time_zone'] = 'Asia/Jakarta'

# line 71-80 Email Settings
# line 413- Backup Settings
# line 543-559 GitLab database settings
# line 566-575 GitLab Redis settings
# line 600-608 GitLab email server settings
# line 1114 GitLab NGINX
# dll
```
> dokumentasi yang lebih lengkap terkait konfigurasi dan berbagai isu umum yang munkin terjadi dapat dibaca [di sini](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md).

Setelah disimpan, eksekusi perintah berikut agar perubahannya diterapkan.

```
sudo gitlab-ctl reconfigure
```

Perintah tersebut akan mengonfigurasi ulang server gitlab dengan konfigurasi yang baru diubah.

## Hapus instalasi GitLab

- Untuk menghapus akun pengguna dan group yang dibuat oleh GitLab, sebelum menghapus paket omnibus gitlab (dengan `dpkg` atau `yum`), eksekusi `sudo gitlab-ctl remove-accounts`
- Hapus semua data omnibus-gitlab `sudo gitlab-ctl cleanse`
- Untuk menghapus instalasi GitLab, dengan mempertahankan data (repositori, database, konfigurasi), jalankan perintah berikut:

```
# Stop gitlab and remove its supervision process
sudo systemctl stop    gitlab-runsvdir
sudo systemctl disable gitlab-runsvdir
sudo rm /usr/lib/systemd/system/gitlab-runsvdir.service
sudo systemctl daemon-reload
sudo gitlab-ctl uninstall

# Debian/Ubuntu
sudo apt remove gitlab-ce
```

- Saya sendiri biasanya lebih memilih install ulang (kalo baremetal), atau destroy VM (kalo VPS)
