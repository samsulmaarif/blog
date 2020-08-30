---
layout: post
title: Install GitLab Runner dengan Ansible
date: '2020-04-10'
author: Samsul Maarif
categories: blog
tags:
- Linux
- GitLab
- DevOps
cover-img:
  - "/img/teluk-penyu-2017.jpg" : "Pantai Teluk Penyu, Cilacap (2017)"
share-img: "/img/teluk-penyu-2017.jpg"
---

Setelah kita selesai menyiapkan server GitLab di postingan sebelumnya [ini](/2020/04/install-gitlab-dengan-ansible.html) dan/atau [ini](/2020/04/install-gitlab-di-Ubuntu-18.04.html), selanjutnya akan kita setup server atau instance untuk digunakan sebagai Runner. Apa itu GitLab Runner?

GitLab Runner merupakan aplikasi sumber terbuka yang digunakan untuk menjalankan *job* dan mengirim kembali hasilnya ke GitLab. Sebagai penghubung dengan GitLab CI, layanan integrasi berkelanjutan disertakan dengan GitLab yang mengoordinasikan *job*.

GitLab Runner ditulis dengan [bahasa pemrograman Go](https://golang.org/), dan dapat dijalankan sebagai aplikasi biner tunggal, tidak ada bahasa pemrograman spesifik yang diperlukan. Didesain untuk berjalan di sistem operasi GNU/Linux, macOS, dan Windows. Mungkin juga bisa berjalan di sistem operasi lain selama Anda dapat *ngompel* biner Go di sana.  

Kapan kita perlu menginstall GitLab Runner sendiri? Nah, ini pertanyaan penting sih sebenarnya.

Iya, kapan sih? Padahal kan kita bisa menggunakan yang ada di GitLab.com gratis pula. Betul, itu gratis. Namun ada batasan waktunya adalah 2000 menit akumulasi dalam sebulan.

Kalau project Anda ada banyak, dan perlu menjalan banyak **job pipeline** per hari yang membutuhkan lebih dari 2000 menit total durasi pipeline job per bulan, ya Anda perlu punya runner sendiri. Apalagi jika Anda menginstall sendiri server GitLab-nya (bukan menggunakan layanan cloud gitlab.com), tentunya perlu membuat runner sendiri. Ya, kan?

Iya in aja deh, biar cepet. hehehe... Oke deh, sudah cukup panjang basa-basinya. Sekarang kita lanjut ke proses instalasi GitLab Runner.

[![Slide Install GitLab Runner](https://i.imgur.com/hHy399J.png)](https://www.slideshare.net/SamsulMaarif15/deploy-multinode-gitlab-runner-in-opensuse-151-instances-with-ansible-automation?ref=https://blog.samsul.web.id/)

Anda boleh baca dulu salindia tersebut (slideshare), itu adalah materi yang pernah saya bawakan saat acara [openSUSE Asia Summit 2019 di Bali](https://events.opensuse.org/conferences/summitasia19/program/proposals/2588).

Untuk mengikuti tutorial ini (ya anggap aja ini tutorial :-D ), saya asumsikan Anda memiliki beberapa komponen berikut:

komponen | spesifikasi
-- | --
VM/VPS | >= 2 buah
vCPU | >= 1 core masing-masing
Memory | >= 1 GB masing-masing
Disk | >= 10 GB masing-masing
OS | Ubuntu 18.04 atau openSUSE Leap 15.1

Nah, asumsinya pula, VM (Virtual Machine)/VPS (Virtual Private Server) atau komputer tersebut telah siap baik dengan ip statis maupun dinamis, baik ip lokal maupun ip publik. Yang akan dilakukan pada masing-masing instance adalah sebagai berikut :

- install GitLab Runner
- install Docker (sebagai executor)
- daftarkan runner ke server GitLab

## Install GitLab Runner dengan Ansible

Ansible akan digunakan sebagai alat untuk menginstall aplikasi-aplikasi tersebut di dalam VM. Supaya proses instalasinya bisa cepat dan gak ribet, apalagi jika VMnya ada banyak, ya kan. Nah, berikut cara installnya (kalo Anda sudah punya Ansible, langkah ini dilewati saja)

```
sudo apt update
sudo apt install -y python-pip git
sudo pip install ansible
```

Sudah, itu aja. Itu kalo laptop Anda pake distro keluarga Debian, seperti Ubuntu, Linux Mint, dll. Untuk proses instalasi Ansible di OS lain silakan baca dokumentasi resminya [di sini](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

Lanjut ya...

Clone repositori berikut yang berisi playbook yang saya buat sebelumnya:

```
git clone https://github.com/samsulmaarif/ansible-gitlab-runner.git
cd ansible-gilab-runner
```

Salin berkas-berkas yang diperlukan dengan perintah `cp`:

```
cp hosts.example hosts
cp groups_vars/all.yml.example groups_vars/all.yml
```

Contoh berkas inventory **hosts** isinya sebagai berikut :

```ini
[runner]
runner01 ansible_host=10.1.1.220
runner02 ansible_host=10.1.1.221
runner03 ansible_host=10.1.1.222
```

Contoh berkas variabel **all.yml**

```yaml
runner_user: gitlab-runner
runner_timezone: Asia/Jakarta
runner_host_user: samsul
runner_registration_token: XXXEy5nG9jHQXXXXXXXX
runner_concurrent: 1
runner_coordinator_url: https://gitlab.com/
runner_description: The runner with the name
runner_executor: docker
runner_docker_image: alpine
runner_tags:
    - dot
    - docker
runner_path: /usr/local/bin/gitlab-runner
additional:
    node_exporter:
        deploy: yes
        version: 0.18.0
        arch: amd64
    docker_exporter:
       deploy: yes
       port: 9323
    ctop:
        deploy: yes
        version: 0.7.2
        arch: amd64
```

Buka instance GitLab Anda, entah itu GitLab.com atau di server Anda sendiri:

1. pada project/group, buka **Settings** > **CI/CD** > Klik tombol `Expand` pada bagian **Runner**,
2. salin token pada **runner registration token**,
3. kembali ke playbook Anda, dan buka berkas `groups_vars/all.yml`,
4. paste/tempel token di sebelah `runner_registration_token`,
5. ubah nilai `runner_coordinator_url` dengan URL GitLab Anda, lalu simpan berkas tersebut.

Gunakan playbook `main.yml` untuk menginstall komponen-komponen di atas.

```
ansible-playbook -i hosts main.yml --ask-sudo-pass
```
ketikkan perintah di atas, lalu Enter, ketikkan password lalu Enter lagi. Anda juga dapat menghilangkan `--ask-sudo-pass` jika Anda sudah mengatur sudo tanpa password.

Jika prosesnya berjalan lancar, sekarang saatnya *runner* kita daftarkan ke instance gitlab. Gunakan playbook `register-runner.yml` untuk mendaftarkannya.

```
ansible-playbook -i hosts register-runner.yml --ask-sudo-pass
```
Setelah prosesnya selesai, periksa kembali instance GitLab kita pada **Settings** > **CI/CD** > **Runner**. Jika Anda melihat ada runner baru di sana, maka proses setup GitLab Runner telah selesai.
