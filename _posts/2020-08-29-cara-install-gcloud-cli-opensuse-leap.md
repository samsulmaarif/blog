---
layout: post
title: Cara Install gcloud CLI di openSUSE Leap 15.2
date: '2020-08-29'
author: Samsul Maarif
categories: blog
tags:
- Linux
- openSUSE
- google-cloud
cover-img:
  - "/img/tugu-malang-2019.jpg" : "Tugu Malang (2019)"
share-img: "/img/tugu-malang-2019.jpg"
---

Postingan kali ini akan sangat singkat, karena merupakan catatan yang dibuat untuk sekedar mendokumentasikan apa yang saya ketikkan saat memasang `gcloud` CLI di laptop saya. Sistem Operasi yang digunakan di laptop saya seperti yant tertulis di judul tulisan ini, yaitu [openSUSE Leap 15.2](https://software.opensuse.org/distributions/leap).

Sebelumnya, mungkin perlu juga saya tulisan apa itu `gloud` dan mengapa saya perlu memasangnya? Oke, jadi, [Google Cloud SDK](https://cloud.google.com/sdk/) merupakan sepaket alat yang digunakan untuk mengelola sumber daya dan aplikasi yang dijalankan di Google Cloud. Nah, jika kita memasang SDK-nya, bukan cuma `gcloud` saja yang terpasang, alat-alat lain seperti `gsutil`, dan `bq` juga terpasang.

Saya perlu memasangnya di laptop saya karena saya membutuhkannya, hehehe.... ya karena saya akan mengelola beberapa aplikasi dan server (VM) yang ada di [Google Cloud Platform (GCP)](/2020/08/opensuse-leap-di-google-cloud-platform.html). Untuk sebagian besar pekerjaan, menggunakan CLI akan mempercepat pekerjaan itu sendiri.

Nah, karena saya menggunakan openSUSE, dan saya lihat tidak ada metode instalasi menggunakan repositori untuk sistem operasi tersebut, maka begini cara saya memasangnya.

```
wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-307.0.0-linux-x86_64.tar.gz
```

Perintah tersebut akan mengunduh file Google Cloud SDK versi 307.0.0, versi terkini pada saat tulisan ini dibuat. Jika Anda membaca tulisan ini jauh setelah 2020, silakan merujuk pada [https://cloud.google.com/sdk/docs/](https://cloud.google.com/sdk/docs/) untuk mengambil URL versi terbarunya di sana.

Setelah proses mengunduh selesai, selanjutnya kita lakukan ekstrak dengan perintah berikut:

```
tar -xzf google-cloud-sdk-307.0.0-linux-x86_64.tar.gz
```

Proses ekstraksi mestinya tidak terlalu lama, kecuali jika Anda menggunakan komputer keluaran tahun 90-an untuk digunakan pada tahun 2020. :D

```
mkdir -p ~/.google
mv google-cloud-sdk ~/.google/sdk
```
Kita eksekusi perintah di atas untuk membuat sebuah folder di `~/.google`, lalu memindahkan folder hasil ekstraksi tadi ke dalam folder `~/.google/sdk`.

Selanjutnya kita eksekusi perintah `install.sh`:

```
~/.google/sdk/install.sh
```

Sesaat setelah kita tekan Enter, akan muncul pertanyaan seperti ini:

```
......

Do you want to help improve the Google Cloud SDK (y/N)?
```

Bisa langsung tekan Enter aja, lalu:

```
......
Modify profile to update your $PATH and enable shell command
completion?

Do you want to continue (Y/n)?
```

Tekan Enter lagi,

```
The Google Cloud SDK installer will now prompt you to update an rc
file to bring the Google Cloud CLIs into your environment.

Enter a path to an rc file to update, or leave blank to use
[/home/samsul/.bashrc]:  
Backing up [/home/samsul/.bashrc] to [/home/samsul/.bashrc.backup].
[/home/samsul/.bashrc] has been updated.

==> Start a new shell for the changes to take effect.


For more information on how to get started, please visit:
  https://cloud.google.com/sdk/docs/quickstarts
```

Sampe di sini, proses instalasi sudah selesai. Namun untuk dapat menggunakannya, kita perlu reload shell, yaitu dengan logout lalu login lagi, atauuuuu... tinggal eksekusi perintah berikut:

```
source .bashrc
```

Dengan begini kita sudah dapat menggunakan perintah `gcloud` di laptop kita. Lebih penting lagi, kita perlu lakukan otentikasi agar terhubung dengan akun GCP kita:


```
gcloud auth login
```

Langkah selanjutnya dapat dibaca di postingan saya yang lain [di sini](/2020/08/opensuse-leap-di-google-cloud-platform.html). Happy hacking int the cloud!

Ingin belajar lebih jauh tentang teknologi Google Cloud? atai ingin menyewa jasa kami? boleh lah intip ke sini --> [https://nacita.id](https://nacita.id/layanan/jasa/)
