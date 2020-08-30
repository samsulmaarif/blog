---
layout: post
title: Membuat CI/CD di GitLab CI
date: '2020-04-13'
author: Samsul Maarif
categories: blog
tags:
- Linux
cover-img:
  - "/img/osas2019-bali.jpg" : "openSUSE Asia Summit 2019, Bali"
share-img: "/img/osas2019-bali.jpg"
---

Sekarang saatnya saya akan membuat sebuah catatan tentang bagaimana kita akan membuat CI/CD (Continuous Integration & Continuous Delivery/Deployment) di GitLab. Ini merupakan contoh sederhana yang mungkin dapat Anda jadikan acuan jika Anda sama sekali baru dalam dunia CI/CD, khususnya di GitLab CI.

## Istilah-Istilah Penting

Hal pertama yang ingin saya jelaskan adalah adanya beberapa istilah yang akan sering kita gunakan, antara lain:

- pipelines
- jobs
- stages

**Pipeline** merupakan komponen tertinggi dari *continuous integration, delivery, dan deployment*. Pipeline terdiri dari:

- **Job** (id: tugas), yang menentukan **apa yang dilakukan**. Contohnya tugas yang *mengompilasi code*, atau melakukan *test code*.
- **Stages** (id: tahap), yang menentukan **kapan job dilakukan** Contohnya, tahap yang menjalankan test setelah tahap kompilasi code dijalankan.

Tugas dieksekusi oleh Runner. Beberapa tugas yang pada tahap yang sama dieksekusi secara paralel, jika tersedia cukup runner untuk menjalankan tugas secara bersaamaan (baca: concurrent).

Jika semua tugas pada sebuah tahap:

- **Sukses**, pipeline berlanjut ke tahap berikutnya.
- **Gagal**, tahap berikutnya (biasanya tidak) deksekusi dan pipeline terhenti.

Pada umumnya, pipeline dieksekusi secara otomatis dan tidak memerlukan intervensi ketika dibuat. Akan tetapi, ada saat di mana Anda dapat secara manual berinteraksi dengan pipeline.

Biasanya, sebuah pipeline berisi empat tahap, dan dieksekusi pada urutan berikut:

- Tahap `build`, dengan tugas bernama `compile`.
- Tahap `test`, dengan tugas bernama `test1` dan `test2`.
- Tahap `staging`, dengan tugas bernama `deploy-ke-staging`
- Tahap `production`, dengan tugas bernama `deploy-ke-production`

Untuk beberapa kasus, mungkin kita akan melewati tahap tertentu, namun umumnya semua tahap tersebut  ada di setiap project.

## Membuat CI/CD

Untuk membuat sebuah proyek CI/CD, kita membutuhkan sebuah proyek (aplikasi, repositori). Saya sudah membuat sebuah repo yang dapat kita gunakan sebagai demonstrasi kali ini.

```bash
wget https://github.com/samsulmaarif/lokada-demo/archive/master.zip
unzip master.zip
cd lokada-demo-master/
```

Sekarang kita memiliki repo, sebuah proyek web yang dibuat dengan **jekyll**. Jekyll merupakan sebuah alat untuk membuat blog/website dari file txt/markdown.

Kita dapat menjalankan proyek tersebut terlebih dahulu di lokal. Namun untuk menjalankannya kita memerlukan aplikasi jekyll terinstall di laptop kita. Proses instalasinya dapat dibaca di [dokumentasi resminya di sini](https://jekyllrb.com/docs/installation/).

Proses instalasinya di laptop saya (os openSUSE Leap 15.1)

```bash
sudo zypper install -t pattern devel_ruby devel_C_C++
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
gem install jekyll bundler
```

Install dependency proyeknya, lalu jalankan :

```
bundle install --path vendor/bundle
bundle exec jekyll serve
```

Aplikasi dapat diakses melalui peramban dengan alamat http://localhost:4000 seperti tampak pada tangkapan layar ini:

![Tangkapan layar jekyll](https://i.imgur.com/SIihxP2.png)

Sekarang kita tahu bahwa aplikasi ini bisa berjalan. Sekarang saatnya kita buat repo di server gitlab, lalu kita push proyek kita ke server. Buat project di GitLab, seperti terlihat di tangkapan layar berikut:

![Buat project di gitlab](https://i.imgur.com/sySMoOh.png)

Setelah proyek dibuat, akan muncul petunjuk cara upload repo kita. Karena saya sudah memiliki file di laptop yang akan diupload, saya memilih opsi *Push an existing folder*

```bash
git init
git remote add origin https://gitlab.com/nacita/lokada-demo.git
git add .
git commit -m "Commit pertama"
git push -u origin master
```

Kira-kira begini cara uploadnya :

<iframe width="560" height="315" src="https://www.youtube.com/embed/rv__Lr9BTEc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Nah, selanjutnya bagian yang menarik adalah membuat konfigurasi CI/CD dengan membuat berkas bernama `.gitlab-ci.yml`. Untuk hal ini, mari kita buat dan isikan sebagai berikut :

```yaml
stages:
  - build
  - test
  - deploy

image: ruby:2.6

cache:
  paths:
  - vendor/

build_jekyll:
  stage: build
  before_script:
    - bundle install --path vendor
  script:
    - bundle exec jekyll build
  artifacts:
    paths:
    - _site
  only:
    - master

test_blog:
  stage: test
  script:
    - bundle exec htmlproofer ./_site --disable-external
  dependencies:
    - build_jekyll
  only:
    - master
  allow_failure: true

deploy_blog:
  stage: deploy
  image: alpine:3.11
  cache: {}
  before_script:
    - apk add --no-cache openssh-client ca-certificates bash rsync
    - eval $(ssh-agent -s)
    - /bin/bash -c 'ssh-add <(echo "$NACITA_SECRET_KEY" | base64 -d)'
  script:
    - rsync -e "ssh -o StrictHostKeyChecking=no" -avHP _site/ $DEPLOY_USER@$DEPLOY_HOST:/home/situs/blog
  dependencies:
    - build_jekyll
  only:
    - master
```

Di situ kita buat tiga tugas (tasks), yaitu `build_jekyll`, `test_blog` dan `deploy_blog`.

- `build_jekyll` berisi tugas untuk mengubah file mentah (.markdown) menjadi file yang bisa digunakan sebagai aplikasi web (.html). Pada `before_script` kita isi supaya dia menginstall *dependency* yang diperlukan, pada `script` berisi perintah untuk dilakukan proses *build* yaitu `bundle exec jekyll build`
- `test_blog` berisi script untuk memeriksa apakah ada kesalahan atau issue pada file yang telah dibuat oleh tugas sebelumnya.
- `deploy_blog` berisi script untuk *mendeploy*, atau mengirimkan hasil *build* tadi ke web server. Untuk keperluan ini saya telah menyiapkan sebuah instance web server untuk **mendeploy** aplikasi ini dengan hostname `blog.nacita.id`.

Oke, terlihat cukup panjang ya? Baiklah, saya mungkin tidak dapat menjelaskan satu per satu fungsi bagian-bagiannya, namun dapat Anda baca dari [sini](https://docs.gitlab.com/ee/user/project/pages/getting_started_part_four.html#practical-example). Tambahkan file tersebut ke dalam proyek, lalu push:

```
git add .gitlab-ci.yml
```

Sebelum Anda melakukan push pembaruan repositori ke server, perlu dilakukan penambahan variabel lingkungan (ENV) proyek di GitLab. Buka proyeknya, di panel sebelah kiri ada menu **Settings** -> **CI/CD** -> **Variables**, tambahkan variabel yang dibutuhkan. Dalam hal ini, variabel yang saya gunakan di dalam berkas `.gitlab-ci.yml` tadi adalah `DEPLOY_USER`, `DEPLOY_HOST`, `NACITA_SECRET_KEY`. Baiklah, saya akan menambahkan sedikit keterangan lagi:

- `DEPLOY_USER`, merupakan nama user untuk login ke server web
- `DEPLOY_HOST`, nama host atau dapat diisi juga dengan alamat ip server web
- `NACITA_SECRET_KEY`, ssh key privat yang telah dienkripsi dengan `base64`.

Kirim perubahan ke server GitLab dengan perintah berikut:

```
git push origin master
```

Kembali ke GitLab, di repositori yang kita buat sebelumnya, di panel kiri cari menu **CI/CD** -> **Pipelines**, Anda akan melihat tampilan seperti berikut:

![Pipeline yang sedang dibuat](https://i.imgur.com/Ozkc8Dv.png)

Setelah beberapa saat akan terlihat apakah sukses, atau gagal.

![](https://i.imgur.com/KHorIGi.png)

Kita juga dapat melihat lebih detail untuk masing-masing stage dan job-nya.

![](https://i.imgur.com/xhpaSBt.png)

Klik pada tiap tahapnya untuk melihat lebih detail. Pastikan semua tahap berjalan sukses yang ditandai dengan pipeline yang berwarna hijau, artinya CI/CD berhasil melakukan semua tahap, termasuk deployment.

![Aplikasi berhasil dideploy](https://i.imgur.com/q3Yt9lf.png)

Sampai di sini proses pembuatan CI/CD sudah selesai. Cobalah membuat perubahan pada project, misal mengubah deskripsi atau menambah postingan baru, lalu push ke gitlab, lihat perubahan setelah dideploy.

Selamat mencoba...!
