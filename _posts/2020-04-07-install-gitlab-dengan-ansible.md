---
layout: post
title: Install GitLab dengan Ansible
date: '2020-04-07'
author: Samsul Maarif
categories: blog
tags:
- Linux
- GitLab
- DevOps
- Ansible
cover-img:
  - "/img/sawah-sidareja.jpg" : "Purwosari, Kedungreja, Cilacap (2019)"
share-img: "/img/sawah-sidareja.jpg"
---

Nah, kemarin sudah kita bahas instalasi GitLab secara manual di [postingan sebelumnya](/2020/04/install-gitlab-di-Ubuntu-18.04.html). Kali ini saya akan tuliskan langkah-langkah instalasi GitLab dengan cara yang lebih simple, yaitu dengan Ansible.

[Ansible](https://www.ansible.com/) adalah sebuah alat otomasi yang dikembangkan oleh RedHat. Beberapa kata kunci yang melekat pada Ansible adalah "automation tool", "configuration management", "infrastructure as code (IaaC)", dan lain-lain. Langsung aja yuks...

## Install Ansible

Saya biasanya menginstall Ansible menggunakan pip, karena biasanya akan dapat yang terbaru:

```
sudo pip install ansible
```

## Siapkan playbook

Nah, daripada bikin dari nol, saya menggunakan role yang sudah ada [yang dibuat oleh Jeff Geerling](https://github.com/geerlingguy/ansible-role-gitlab) sebagai submodule di repo saya.

Clone repo berikut secara rekursif agar submodulenya juga ikut terunduh:

```
git clone --recursive https://github.com/samsulmaarif/ansible-gitlab.git
cd ansible-gitlab
```

Salin berkas contoh `hosts` lalu tambahkan alamat ip calon server GitLab:

```
cp hosts{.example,}
vim hosts
```

Salin berkas variabelnya lalu sunting dan sesuaikan konfigurasinya:

```
cp vars/main.yml{.example,}
vim vars/main.yml
```

Lengkapnya dapat dibaca [di sini](https://github.com/geerlingguy/ansible-role-gitlab/blob/master/README.md#role-variables) contoh konfigurasinya.

Dengan asumsi servernya sudah siap, lalu jalankan playbooknya dengan perintah berikut, pastikan tidak ada yang galat yang muncul:

```
ansible-playbook -i hosts main.yml
```

Berikut screencast saya buat dengan asciinema.

[![asciicast](https://asciinema.org/a/L3dKw8IxNvnPb4haglfk26o2L.svg)](https://asciinema.org/a/L3dKw8IxNvnPb4haglfk26o2L)

Setelah proses instalasi selesai, akses hasil instalasi gitlab kita melalui browser, baik dengan alamat IP atau nama host-nya.

Selamat mencoba.
