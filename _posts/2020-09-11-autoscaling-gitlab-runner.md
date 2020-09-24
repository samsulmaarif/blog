---
layout: post
title: Cara Install Autoscaling GitLab Runner yang Hemat Biaya
date: '2020-09-11'
author: Samsul Maarif
categories: blog
tags:
- Linux
- openSUSE
- GitLab
cover-img:
  - "/img/taman-kendedes-2018.jpg" : "Taman Kendedes, Malang (2018)"
share-img: "/img/taman-kendedes-2018.jpg"
---

Ngomong-ngomong soal GitLab tidak lepas soal CI/CD (Continuous Integration/Continuous Delivery/Deployment).  Nah, kebetulan Di tempat saya bekerja menggunakan gitlab-runner. GitLab Runner merupakan salah satu fitur yang sangat bermanfaat untuk menjalankan CI/CD.

Saya tidak akan menjelaskan apa itu CI/CD di sini (mungkin akan saya jelaskan di postingan yang lain). Hanya akan saya jelaskan mengapa kita membutuhkan GitLab Runner.  Sebagai pengguna gitlab.com Kita sebenarnya sudah bisa menggunakan *Shared Runner* bawaannya gitlab secara gratis, namun ternyata fitur standar bawaan tersebut  ternyata ada kekurangannya yaitu ada batasan menit di mana kita dapat menjalankan Runner di sana.  

Bahkan berdasarkan [pengumuman terbaru dari GitLab](https://about.gitlab.com/releases/2020/09/01/ci-minutes-update-free-users/) mulai 1 Oktober 2020 gitlab menurunkan batas menit untuk Runner bawaannya mereka dari yang semula 2000 menit menjadi 400 menit per Group (organisasi). Dua ribu apalagi 400 menit itu untuk menjalankan CI job  sangat terbatas.  Hanya dalam beberapa hari batasan itu itu segera terlampaui. Nah salah satu untuk menghilangkan batas tersebut adalah dengan kita memasang Runner kita sendiri.  

Pada awalnya Kami menggunakan sebuah VM khusus di GCP untuk dipasang GitLab Runner. Server tersebut berjalan secara terus menerus sepanjang hari dan dapat menangani job lebih banyak dari sebelumnya.  Akan tetapi, ternyata cost-nya juga lebih banyak dari sebelumnya.

Pada suatu hari Bapak CTO kami memberikan sebuah referensi dan meminta tim DevOps Yang pada saat itu beranggotakan saya sendiri untuk melakukan riset. “Apakah hal ini bisa kita implementasikan?” tanya beliau.

{: .box-success }
Ini adalah catatan penting, khususnya untuk saya sendiri. Lebih dari setahun kayaknya saya memikirkan untuk "saya ingin menulis tentang ini." Namun rasanya, ini adalah saat yang paling tepat untuk menulisnya.

Tidak lain [karena ini](https://adinusa.id/blogs/agenda-kegiatan-reuni-virtual-nolsatu-dan-peresmian-adinusa), hehehe.... Ya, menjadi penyaji akan selalu memotivasi saya untuk menulis, belajar, dan melakukan riset kecil-kecilan.

Baiklah rasanya tidak perlu lebih panjang lagi catatan pembuka ini langsung saja kita ke intinya.

## Persyaratan

Untuk mengikuti tutorial ini sebaiknya temen-temen paham apa itu GitLab, CI/CD, Docker, dan memiliki akun GCP yang aktif.

## Buat GCP Project

Buat sebuah Project baru di gcp namanya bebas dalam hal ini nama yang akan saya gunakan adalah Nacita Nolsatu, dengan ID `nacita-nolsatu`. ID tersebut akan kita gunakan pada langkah berikutnya.

## Orchestrator VM

Selanjutnya kita perlu membuat sebuah instance (VM), yang akan kita gunakan sebagai orkestrator. Ada dua cara untuk membuat sebuah instance GCP, cara pertama adalah menggunakan [gcloud CLI](/2020/08/cara-install-gcloud-cli-opensuse-leap.html) sedangkan cara kedua adalah menggunakan Google Cloud console.  

Untuk cara pembuatan VM dengan gcloud CLI dapat di baca [di sini](/2020/08/opensuse-leap-di-google-cloud-platform.html)

Melalui Google Cloud Console saya sempet bikin videonya :

<iframe width="560" height="315" src="https://www.youtube.com/embed/uIcEBukysTQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Cuma sayangnya, jika menggunakan Console tersebut saya tidak dapat membuat VM dengan sistem operasi openSUSE 15.2. Jadi saya buat lewat gcloud saja. Perintahnya sebagai berikut:

```
gcloud compute instances create nacita-runner \
    --image-family=opensuse-leap \
    --image-project=opensuse-cloud \
    --project=nacita-nolsatu \
    --zone=asia-southeast2-a \
    --machine-type=f1-micro \
    --boot-disk-size=20GB
```

Penjelasan detailnya tentang perintah tersebut dapat dibaca di [tulisan saya yang lain](/2020/08/opensuse-leap-di-google-cloud-platform.html). Namun yang perlu saya garis bawahi di sini adalah penggunaan tipe mesin `f1-micro`, adalah tipe mesin terkecil yang tersedia di GCP.

Alasan dari penggunaan tipe mesin tersebut adalah kita minimalisir seminimal mungkin penggunaan sumberdaya yang digunakan, tentunya berdampak pada biaya yang juga rendah. Toh, VM ini hanya akan digunakan sebagai orkestrator dan tidak membutuhkan terlalu banyak sumberdaya untuk menjalankannya.

## GCP Firewall rules

Firewall rule ini diperlukan untuk komunikasi antar orkestrator dan machine (VM/runner) yang berjalan.

```
gcloud compute firewall-rules create docker-machines \
    --direction=INGRESS --priority=1000 --network=default \
    --action=ALLOW --rules=tcp:2376 --source-ranges=0.0.0.0/0 \
    --project=nacita-nolsatu
```

Jika firewall rule ini tidak dibuat (tidak ada), maka orkestrator akan gagal membuat machine (VM/runner) karena  tidak dapat menghubungi VM yang dimaksud.

## Pasang Komponen yang Diperlukan

Komponen tersebut antara lain :
1. Docker, --> [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
2. Docker Machine, --> [https://docs.docker.com/machine/install-machine/](https://docs.docker.com/machine/install-machine/)
3. GitLab Runner, --> [https://docs.gitlab.com/runner/install/](https://docs.gitlab.com/runner/install/)

Nah, jika Anda juga menggunakan openSUSE Leap, saya sudah menyiapkan sebuah script untuk memasang semua komponen tersebut. Pertama SSH dulu ke dalam VM orkestrator kita.

dengan `gcloud`, dalam hal ini VM saya bernama `nacita-runner`

```
gcloud compute ssh nacita-runner
```

**atau** SSH biasa jika kita sudah dapat alamat IP-nya, dan ssh key kita sudah terdaftar di VM.

```
ssh user@ip-vm
```

Setelah berhasil login, download script-nya lalu jalankan:

```
curl -L --output install-gitlab-runner.sh https://gist.githubusercontent.com/samsulmaarif/d11595c65dc69f8af505a11e464461b1/raw/install-gitlab-runner.sh
chmod +x install-gitlab-runner.sh
sudo ./install-gitlab-runner.sh
```

Tunggu beberapa saat hingga proses instalasinya selesai.

## Daftarkan Runner ke GitLab

Untuk mendaftarkan Runner tersebut, berikut langkahnya:

1. Ambil token dari pengaturan Group atau Project di GitLab,  
di menu *Settings* -> *CI/CD* -> klik **Expand** pada *Runners* -> salin kode token.
2. Jalankan perintah berikut:
```
sudo gitlab-runner register
```
3. Masukkan URL Instance GitLab, dalam hal ini adalah `https://gitlab.com/`  
4. Masukkan kode token yang sudah diambil pada langkah 1 tadi.
5. Masukkan deskripsi runner. Nantinya dapat diubah melalui antarmuka pengguna GitLab.
6. Masukkan tag, dipisahkan dengan koma (,). Tag ini nantinya digunakan sebagai penanda saat menjalankan CI/CD.
7. Gunakan `docker+machine` sebagai executor,
8. Jika diminta untuk menentukan default image, masukkan nama image yang dikehendaki, atau ketikkan `alpine` jika tidak yakin.

## Cache server

Nah, untuk urusan cache, kita dapat menggunakan S3-nya AWS, atau GCS-nya Google. Tentunya karena dari awal kita menggunakan Google, catatan selanjutnya pun akan dibahas bagaimana membuat dan menggunakan Google Cloud Storage (GCS). Langkah berikut dapat dieksekusi di terminal di laptop yang sudah [terinstall gcloud sdk](/2020/08/cara-install-gcloud-cli-opensuse-leap.html) dan terhubung ke akun google kita, maupun di [google cloud shell](https://cloud.google.com/shell/).

Pertama kita buat embernya (bucket):

```
gsutil mb -l asia gs://nacita-cache
```

Sedikit penjelasan nih mengenai opsi yang digunakan:
- opsi `-l asia` kita memilih lokasi ember terdekat yaitu di `asia`
- pada bagian terakhir, `nacita-cache` adalah nama storage bucket-nya.

Selanjutnya kita buat `service-account` untuk otentikasi ke GCS tersebut nantinya:

```
gcloud iam service-accounts create gitlab --display-name gitlab --project=nacita-nolsatu
```

Setelah `service-account` dibuat, beri `iam-policy` dengan akses role sebagai `storage.admin` agar dapat menulis ke GCS:

```
export PROJECT=nacita-nolsatu
export SA_EMAIL=$(gcloud iam service-accounts list \
  --filter="displayName:gitlab" --format='value(email)')
gcloud projects add-iam-policy-binding $PROJECT  --role roles/storage.admin \
  --member serviceAccount:$SA_EMAIL
```

Langkah yang tidak kalah pentingnya adalah kita buat berkas key berupa json:

```
gcloud iam service-accounts keys create gitlab-sa.json --iam-account $SA_EMAIL
```

File bernama `gitlab-sa.json` tersebut nantinya akan kita upload ke orkestrator.

## Konfigurasi runner

Berkas konfigurasinya berada di `/etc/gitlab-runner/config.toml`. Berkas tersebut otomatis dibuat sesaat setelah kita melakukan [proses registrasi](#daftarkan-runner-ke-gitLab) di bagian sebelumnya. Namun beberapa bagian perlu kita Konfigurasi secara manual.

Buka berkas tersebut dengan editor teks favorit :

```
sudo vim /etc/gitlab-runner/config.toml
```

> Ganti `vim` dengan editor favorit Anda, jika Anda kesulitan menggunakan **VIM**.

### atur Concurent runner

`concurent` mendefinisikan berapa jumlah runner yang boleh berjalan secara bersamaan. Atur sesuai kebutuhan, dalam hal ini saya buat `10`.

### atur Konfigurasi cache

Pada bagian `runners.cache` kita akan menggunakan GCS yang mana bucket-nya sudah kita buat tadi. Kurang lebih seperti ini :

```
[runners.cache]
  Type = "gcs"
  Path = "gitlab"
  Shared = true
  [runners.cache.s3]
  [runners.cache.gcs]
    CredentialsFile = "/etc/gitlab-runner/gitlab-sa.json"
    BucketName = "nacita-cache"
```

Di sini diasumsikan kita sudah mengupload file `gitlab-sa.json` ke direktori `/etc/gitlab-runner`.

### atur konfigurasi autoscaling

Sisanya, kita sesuaikan konfigurasi autoscaling sesuai kebutuhan kita:

```
[runners.machine]
  IdleCount = 0
  MachineDriver = "google"
  MachineName = "nacita-runner-playground-%s"
  MachineOptions = [
    "google-project=nacita-nolsatu",
    # tentukan tipe mesin, untuk job umumnya dengan n1-standard-1 cukup
    "google-machine-type=n1-standard-1",
    # VM tag digunakan untuk memudahkan penandaan jika perlu mengubah firewall di VPC nantinya
    "google-tags=gitlab-runner,docker,nacita-runner",
    # preemtible vm kita aktifkan agar VM yang tidak digunakan akan otomatis dimatikan
    "google-preemptible=true",
    # samakan dengan zona yang digunakan oleh orkestrator
    "google-zone=asia-southeast2-a",
    "google-use-internal-ip=true",
    "engine-registry-mirror=https://mirror.gcr.io",
    # yang ini opsional ya, nilai bawaannya adalah 10, artinya 10Gb ukuran diska VMnya
    "google-disk-size=20"
  ]
  [[runners.machine.autoscaling]]  # Penentuan periode dengan beberapa pengaturan
    Periods = ["* * 7-17 * * mon-fri *"] # Hari kerja dari jam 7 sampe 17 WIB
    IdleCount = 0
    IdleTime = 600
    Timezone = "Asia/Jakarta"
  [[runners.machine.autoscaling]]
    Periods = ["* * * * * sat,sun *"] # konfigurasi selama akhir pekan
    IdleCount = 0
    IdleTime = 300
    Timezone = "Asia/Jakarta"

```

Sepertinya akan sangat panjang kalau saya jelaskan per baris... hahahaha, jadi saya kasih komentar saja di codeblock di atas.

## Rangkuman

Oke, supaya lebih jelas konfigurasi lengkapnya seperti berikut:

```
concurrent = 10
check_interval = 0

[[runners]]
  name = "Nacita GitLab Runner "
  url = "https://gitlab.com/"
  token = "XXXXXXXXXXXXXXXXXX"
  executor = "docker+machine"
  [runners.docker]
    tls_verify = false
    image = "alpine"
    privileged = false
    disable_cache = false
    volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
    shm_size = 0
  [runners.cache]
    Type = "gcs"
    Path = "gitlab"
    Shared = true
    [runners.cache.s3]
    [runners.cache.gcs]
      CredentialsFile = "/etc/gitlab-runner/gitlab-sa.json"
      BucketName = "nacita-cache"
  [runners.machine]
    IdleCount = 0
    MachineDriver = "google"
    MachineName = "nacita-runner-playground-%s"
    MachineOptions = [
      "google-project=nacita-nolsatu",
      "google-machine-type=n1-standard-1",
      "google-tags=gitlab-runner,docker,nacita-runner",
      "google-preemptible=true",
      "google-zone=asia-southeast2-a",
      "google-use-internal-ip=true",
      "engine-registry-mirror=https://mirror.gcr.io",
      "google-disk-size=20"
    ]
    [[runners.machine.autoscaling]]
      Periods = ["* * 7-17 * * mon-fri *"]
      IdleCount = 0
      IdleTime = 600
      Timezone = "Asia/Jakarta"
    [[runners.machine.autoscaling]]
      Periods = ["* * * * * sat,sun *"]
      IdleCount = 0
      IdleTime = 300
      Timezone = "Asia/Jakarta"
```

Simpan berkas konfigurasi tersebut, lalu lakukan restart:

```
sudo gitlab-runner restart
```

meskipun katanya, perubahan dalam konfigurasi tersebut secara otomatis terbaca oleh gitlab-runner. Cuma biasanya kita kurang yakin, jadi direstart aja... hahahaha.

{: .box-success }
Oiya, untuk temen-temen yang tertarik dan serius ingin belajar DevOps, GitLab, CI/CD silakan cek poster berikut:

![Izu Zero-One](https://wiki.tvnihon.com/w/images/a/a9/KRZeroOnecast03.png){: .mx-auto.d-block :}

Eh, maaf salah paste gambar, maksudnya yang ini :

![Poster Training Intermediate GitLab CI](https://nacita.id/img/poster/intermediate-gitlab-ci.jpg){: .mx-auto.d-block :}

Atau klik aja pranala ini --> [https://nacita.id/training/gitlab-intermediate/](https://nacita.id/training/gitlab-intermediate/)


## Referensi
- [https://verkoyen.eu](https://verkoyen.eu/blog/2018/08/scaling-gitlab-runner-on-google-cloud-platform)
- [https://docs.gitlab.com/runner/configuration/advanced-configuration.html](https://docs.gitlab.com/runner/configuration/advanced-configuration.html)
- [https://docs.gitlab.com/runner/configuration/autoscale.html](https://docs.gitlab.com/runner/configuration/autoscale.html)
