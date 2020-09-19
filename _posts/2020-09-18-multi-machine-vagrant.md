---
layout: post
title: Multi Machine Vagrant dengan Vagrantfile Tunggal
date: '2020-09-18'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Vagrant
- CentOS
cover-img:
  - "/img/tefa-smk-yadika-bangil-2019-3bw.jpg" : "Teaching Factory, SMK Yadika Bangil (2019)"
share-img: "/img/tefa-smk-yadika-bangil-2019-3bw.jpg"
---

Vagrant merupakan sebuah tools yang saya gunakan untuk provisioning Virtual Machine, khususnya untuk environment lokal di laptop. Vagrant juga dapat digunakan untuk provisioning VM dengan beberapa provider, antara lain **VirtualBox**, **VMware**, **Docker**, **Hyper-V**, maupun **[custom](https://www.vagrantup.com/docs/plugins/providers)**.

Saya sendiri menggunakan [provider Libvirt](/2020/09/menjalankan-vagrant-libvirt-di-opensuse-leap.html) :D

Oke, salah satu alasan kenapa tulisan ini saya buat karena saya sendiri bingung membaca [dokumentasi resmi dari Vagrant](https://www.vagrantup.com/docs/multi-machine). Jadi saya sendiri perlu mencoba-coba baru paham cara kerjanya.

Skenarionya begini, saya sedang membutuhkan 3 buah VM dengan dengan OS CentOS 8, sudah terinstall Docker, dengan masing-masing VM terdapat ip private. Nah, dengan skenario tersebut, saya ingin dapat diprovision dengan satu kali perintah.

## Tanpa multi-machine

Tanpa skenario multi-machine, saya mungkin dapat menggunakan beberapa folder dengan struktur berikut:

```
vagrant $ tree
.
├── manager
│   ├── install-docker.sh
│   └── Vagrantfile
├── worker01
│   ├── install-docker.sh
│   └── Vagrantfile
└── worker02
    ├── install-docker.sh
    └── Vagrantfile
```

Dengan masing konten `Vagrantfile` sebagai berikut (tentunya dengan hostname yang berbeda):

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_version = "1905.1"
  config.vm.hostname = "manager"
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048
    libvirt.cpus = 2
    libvirt.nested = true
  end
  config.vm.provision "shell", path: "install-docker.sh"
end
```

dan berkas `install-docker.sh` seperti ini:

```bash
#!/usr/bin/env bash
timedatectl set-timezone Asia/Jakarta
dnf install -y https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
dnf install docker-ce -y
systemctl enable docker
systemctl start docker
usermod -aG docker vagrant
```

dan jalankan script berikut di parent foldernya:

```
for t in manager worker01 worker02; do cd $t && vagrant up && cd ..; done
```

Mesin pun dapat terprovision dan goal tercapai. Namun bukan ini yang saya inginkan, tetapi saya ingin dapat dicapai dengan 1 file `Vagrantfile`.

## dengan Multi-machine

Jadi, akhirnya setelah baca-baca dokumentasinya, saya sendiri gak langsung paham. Mungkin karena saking bodohnya saya. Setelah saya coba-coba akhirnya saya menemukan caranya.

Buat sebuah folder baru, sebuah file `Vagrantfile`, dan script `install-docker.sh` (yang sama dengan sebelumnya).

```
mkdir runner
cd runner
vim Vagrantfile
```

Isi file `Vagrantfile` tersebut adalah sebagai berikut:

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.box_version = "1905.1"
  config.vm.provision "shell", path: "install-docker.sh"
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 1024
    libvirt.cpus = 1
    libvirt.nested = true
  end

  config.vm.define "manager" do |manager|
    manager.vm.hostname = "manager"
    manager.vm.network "private_network", ip:"10.2.2.5"
  end

  config.vm.define "node01" do |node01|
    node01.vm.hostname = "node01"
    node01.vm.network "private_network", ip:"10.2.2.6"
  end

  config.vm.define "node02" do |node02|
    node02.vm.hostname = "node02"
    node02.vm.network "private_network", ip:"10.2.2.7"
  end

end
```

Pada file tersebut dapat kita lihat bahwa konfigurasinya terdapat beberapa bagian. Bagian global dengan prefix `config`, dan bagian spesifik masing-masing machine terdapat pada `config.vm.define`.

Konfigurasi globalnya menunjukkan sistem operasi yang digunakan, yaitu pada `config.vm.box` dengan nilai `centos/8`, termasuk versinya. Juga ada script provisioner `install-docker.sh`, dan konfigurasi provider agar menggunakan tipe/spesifikasi mesin, terlihat pada blok `config.vm.provider :libvirt` dan seterusnya.

Nah, konfigurasi global tersebut juga dapat kita terapkan secara spesifik, misalnya kita ingin sang `manager` kita ingin spesifikasinya lebih besar, misalnya sebagai berikut:

```
config.vm.define "manager" do |manager|
  manager.vm.hostname = "manager"
  manager.vm.network "private_network", ip:"10.2.2.5"
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048
    libvirt.cpus = 2
    libvirt.nested = true
  end
end
```

Konfigurasikan sesuai kebutuhan.

Struktur direktorinya seperti ini:

```
runner $ tree
.
├── install-docker.sh
└── Vagrantfile
```

Selanjutnya tinggal kita jalankan `vagrant up`. Untuk lebih jelasnya dapat dilihat pada screencast di asciinema.org berikut:

[![asciicast](https://asciinema.org/a/QrHfLKAhLUUQt3Cz62UnGvAnI.svg)](https://asciinema.org/a/QrHfLKAhLUUQt3Cz62UnGvAnI){: .mx-auto.d-block :}

Hasilnya pun memuaskan, seperti yang saya harapkan.

![Screenshot-20200919205106-570x591](https://user-images.githubusercontent.com/1231314/93668914-26cc3680-faba-11ea-94f4-61f67a5924d9.png){: .mx-auto.d-block :}


Demikian catatan singkat ini, semoga bermanfaat.
