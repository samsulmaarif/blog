---
layout: post
title: Cara Menginstall Python 3.6 di Debian 9 (stretch)
date: '2020-10-25'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Debian
cover-img:
  - "/img/kamen-rider-kuuga.jpg" : "Kamen Rider Kuuga, by TOEI Company"
share-img: "/img/kamen-rider-kuuga.jpg"
---

Debian 9 (stretch) secara default membawa `python2.7` (default) dan `python3.5`. Saya sedang melakukan instalasi aplikasi yang ternyata membutuhkan Python versi 3.6. Pada beberapa tutorial di internet, menyarankan untuk menggunakan repo debian testing. Namun saat tulisan ini dibuat, saya cek repo debian testing memiliki python versi 3.8, sedangkan versi tersebut *ketinggian*.

Oke, jadi berikut catatan saya saat memasang python versi 3.6 di Debian 9. Oiya, pemasangan ini tidak menghapus atau menggantikan versi python yang sudah terinstall.

Langkah pertama ini adalah memasang paket-paket yang dibutuhkan, antara lain berupa paket `-dev` dan beberapa paket lain.

```
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev\
   libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev \
   libncursesw5-dev xz-utils tk-dev
```

Unduh berkas kode sumber python 3.6 dari web resminya, lalu ekstrak.

```
wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tgz
tar -xzf Python-3.6.9.tgz
```

Pindah ke direktori baru hasil ekstrak tersebut.

```
cd Python-3.6.9
```

Jalankan perintah berikut untuk mengonfigurasi sekaligus memeriksa apakah ada pustaka yang kurang atau tidak.

```
./configure --enable-loadable-sqlite-extensions
```

Jika perintah tersebut tidak menampilkan pesan error, kita bisa lanjut ke langkah berikutnya, lakukan kompilasi dengan perntah berikut:

```
make -j2
```

Angka 2 setelah `-j` pada perintah tersebut merupakan jumlah core CPU pada mesin yang sedang kita gunakan. Anda dapat mengubahnya sesuai jumlah core yang agar maksimal proses kompilasinya. Semakin banyak core prosesor yang digunakan tentunya prosesnya akan lebih cepat.

Langkah berikutnya adalah proses instalasi ke dalam sistem, oleh karenanya membutuhkan akses root (dengan sudo):

```
sudo make altinstall
```

Perhatikan bahwa kita menggunakan `altinstall` di sini, perintah tersebut tidak menghapus/menghilangkan versi python yang terpasang. Namun, jika Anda ingin mengganti versi yang terpasang dapat menggunakan `make install` meskipun **saya sangat tidak merkomendasikannya**.

Dan saat proses instalasi sudah selesai, kita dapat memeriksanya dengan mengetikkan perintah ini:

```
python3.6 -V
```

Kita juga dapat melihatnya dengan mengetikkan `python` lalu diikuti `[TAB]` di keyboard, maka akan muncul seperti ini:

```
$ python<TAB>
python             python2.7          python3.5          python3.6          python3.6m-config
python2            python3            python3.5m         python3.6m         python3m
```

Selanjutnya kita dapat menggunakannya dalam sebuah project dengan membuat _virtual environment_:

```
cd /lokasi/project
python3.6 -m venv venv
```

Aktifkan, lalu siap digunakan.

```
source venv/bin/activate
pip install requests
```

Kini kita dapat menjalankan project/aplikasi python dengan versi yang kita inginkan.

## Kesimpulan

Memasang Python 3.6 di Debian 9 cukuplah mudah dengan mengikuti langkah di atas. Penggunakan _virtual environment_ akan mengisolasi versi python pada tiap-tiap project yang kita kerjakan. Beberapa project dengan versi python-nya masing-masing dapat berjalan pada mesin yang sama.

Demikian catatan kali ini, semoga bermanfaat.

## Referensi
- [https://stackoverflow.com](https://stackoverflow.com/questions/45954528/pip-is-configured-with-locations-that-require-tls-ssl-however-the-ssl-module-in#49296846)
- [https://blog.eldernode.com/python-3-6-installation-tutorial-in-debian-9/](https://blog.eldernode.com/python-3-6-installation-tutorial-in-debian-9/)

{: .box-success }
Ingin belajar lebih jauh tentang server Linux & teknologi *Container*? atau ingin menyewa jasa kami? boleh lah intip ke sini --> [https://nacita.id](https://nacita.id/layanan/jasa/)
