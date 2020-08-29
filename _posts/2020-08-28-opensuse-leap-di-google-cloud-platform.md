---
layout: post
title: openSUSE Leap 15.2 di Google Cloud Platform (GCP)
date: '2020-08-28'
author: Samsul Maarif
categories: blog
tags:
- Linux
- openSUSE
- google-cloud
bigimg:
  - "/img/ipin-junior-sysadmin-2018-11-27.jpg" : "Ipin Junior SysAdmin (2018)"
share-img: "/img/ipin-junior-sysadmin-2018-11-27.jpg"
---
Sebagai pengguna openSUSE, saya kadang merasa penasaran apakah bisa saya menggunakan os ini di langit sebagai server. Yang saya maksud *di langit* di sini adalah Google Cloud Platform (GCP), karena saya juga pengguna produk google tersebut di beberapa proyek milik klien. Namun melihat daftar templat citra (image template) di [Google Cloud Console](https://console.cloud.google.com/compute/images) hanya tersedia versi berbayarnya, yaitu SLES (SUSE Linux Enterprise Server).

Dari rasa penasaran tersebut, akhirnya suatu ketika saya iseng googling dan menemukan [sebuah postingan blog](http://sellingfreesoftwareforaliving.blogspot.com/2018/05/opensuse-leap-423-on-google-cloud.html) yang menuliskan kita dapat mendeploy openSUSE Leap di GCP. Di situ disebutkan bahwa openSUSE Leap memang tidak tersedia sebagai citra publik di GCP. Namun, dia terdaftar dalam kategori [**Community Supported images**](https://cloud.google.com/compute/docs/images#community_supported_images), atau citra yang tersedia atau didukung oleh komunitas.

Pada penulisan postingan ini tentunya perlatan yang diperlukan adalah akun GCP yang aktif, dan pengetahuan tentang GCP itu sendiri dan openSUSE. Saya menggunakan akun dengan credit gratis $300 USD dari Google, Anda juga dapat mengambilnya dengan menyiapkan kartu kredit (saya menggunakan [Jenius](https://www.jenius.com/)) lalu mendaftar melalui [https://cloud.google.com/free/](https://cloud.google.com/free/).

Install terlebih dahulu `gcloud` di terminal, saya sendiri lebih menyukai pekerjaan yang dapat dilakukan di terminal. Jika belum silakan baca panduannya di sini [https://cloud.google.com/sdk/docs/](https://cloud.google.com/sdk/docs/).


```
gcloud auth login
```

Lakukan proses otentikasi dengan perintah di atas agar lingkungan CLI kita terhubung dengan akun GCP. Setelah menekan Enter akan muncul pesan seperti `Go to the following link in your browser:`, buka link tersebut melalui peramban favorit Anda, login dengan akun google, salin kode verifikasi, tempelkan kembali ke terminal.

Setelah otentikasi berhasil, lakukan verifikasi dengan mengetikkan :

```
gcloud projects list
```

Eh, perintah tersebut sebenarnya untuk melihat project apa saja yang ada di akun kita. Misalnya dalam hal ini berikut beberapa project yang ada di akun saya:

```
PROJECT_ID              NAME              PROJECT_NUMBER
nacita-dev              nacita-dev        35497xxxxxxx
nacita-nolsatu          nacita-nolsatu    15978xxxxxxx
```

Pilih project yang akan digunakan, dalam hal ini saya akan menggunakan `nacita-nolsatu`:

```
gcloud config set project nacita-nolsatu
```

Untuk dapat melihat image openSUSE yang tersedia di GCP, kita dapat menggunakan perintah berikut:

```
gcloud compute images list --project opensuse-cloud --no-standard-images
```

hasilnya akan sebagai berikut:

```
NAME                          PROJECT         FAMILY         DEPRECATED  STATUS
opensuse-leap-15-1-v20190618  opensuse-cloud  opensuse-leap              READY
opensuse-leap-15-2-v20200702  opensuse-cloud  opensuse-leap              READY
opensuse-leap-15-v20181106    opensuse-cloud  opensuse-leap              READY
```

Selanjutnya kita dapat membuat *instance (VM)* dengan image openSUSE Leap terbaru (dalam hal ini 15.2 pada saat tulisan ini dibuat) dengan perintah berikut:

```
gcloud compute instances create nacita-runner \
    --image-family=opensuse-leap \
    --image-project=opensuse-cloud \
    --project=nacita-nolsatu \
    --zone=asia-southeast2-a \
    --machine-type=f1-micro \
    --boot-disk-size=20GB
```

Perintah tersebut akan membuat sebuah *instance* dengan rincian sebagai berikut:

- Opsi `--image-family` dan `--image-project` mengacu pada output perintah sebelumnya, karena kita akan membuat VM openSUSE Leap.
- Saat ini infrastruktur GCP telah tersedia di Indonesia, oleh karena itu saya menggunakan `asia-southeast2-a` (Jakarta) untuk opsi `--zone`.
- Tipe mesin yang saya gunakan adalah `f1-micro` merupakan tipe mesin terkecil yang tersedia di GCP dengan spesifikasi 1 vCPU, 0.6 GB memory.
- Opsi `--boot-disk-size`, sudah cukup jelas ya.


Proses pembuatan VM sudah selesai, gak percaya? coba kita cek:

```
gcloud compute instances list
```

hasilnya? begini kira-kira:

```
NAME           ZONE               MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP   STATUS
nacita-runner  asia-southeast2-a  f1-micro                   10.184.0.2   34.101.112.6  RUNNING
```

Oke, sekarang saatnya kita coba:

```
samsul@piccolo ~> gcloud compute ssh nacita-runner
No zone specified. Using zone [asia-southeast2-a] for instance: [nacita-runner].
Warning: Permanently added 'compute.1024828835942081102' (ECDSA) to the list of known hosts.
openSUSE Leap 15.2 x86_64 (64-bit)

As "root" use the:
- zypper command for package management
- yast command for configuration management

Have a lot of fun...
samsul@nacita-runner:~>
```

Nah, sampe di sini kita sudah dapat menggunakan VM openSUSE Leap 15.2 kita di GCP. Langkah selanjutnya terserah Anda, apakah akan menginstall web-server atau apapun. Bahkan Desktop Environment, tapi untuk apa?
