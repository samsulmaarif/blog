---
layout: post
title: Menjalankan Vagrant Provider Libvirt di openSUSE Leap 15.2
date: '2020-09-08'
author: Samsul Maarif
categories: blog
tags:
- Linux
- openSUSE
- Vagrant
cover-img:
  - "/img/tugu-malang-2019.jpg" : "Tugu Malang (2019)"
share-img: "/img/tugu-malang-2019.jpg"
---

Melihat pak [Andi Sugandi](facebook.com/andisugandi) menggunakan vagrant untuk menyediakan lab VM saat beliau mengajar, saya pun jadi penasaran. Karena *provisioning* VM menggunakan vagrant ini sepertinya mengasyikkan, terutama karena..... Nanti deh saya ceritain apa, dan mengapa ini penting saya utarakan.

## Apa itu Vagrant?

|![Tampilan Website Vagrant](https://i.imgur.com/HxvwbBG.png)|
|Tampilan Website Vagrant|

Vagrant merupakan sebuah perangkat lunak yang digunakan untuk penyiapan (provisioning) lingkungan virtual, misalnya menggunakan VirtualBox, vmware, dan lain-lain, sehingga kita dapat membuat lingkungan development secara portabel, konsisten, dan lebih fleksibel.

Lalu mengapa saya membutuhkan ini? Kenapa ya? saya kan bukan developer, hehehee. Oke, Iya, pekerjaan saya lebih banyak berurusan dengan infra (server, network, dan cloud provider). Namun justru di situ, karena Vagrant ini juga berurusan dengan Mesin Virtual (VM), saya pun untuk hal tertentu membutuhkannya.

Misalnya, untuk keperluan demo pada saat mengajar, atau saat mencoba/riset akan sebuah aplikasi yang akan dipasang di server. Pada titik tertentu saya perlu menyiapkan serbuah VM untuk testing, dan VM tersebut dapat disiapkan dengan secepat kilat ketika saya menggunakan Vagrant. Ini yang membuat Vagrant menjadi menarik, hehehe...

## Vagrant Provider

Nah, ini yang menjadi alasan kenapa saya menulis postingan ini adalah.... jeng jeng jeng... Libvirt. Libvirt merupakan sebuah **Virtualization API** (demikian singkatnya) favorit saya untuk saya jalankan di laptop untuk membuat VM. Pembaca yang budiman mungkin sudah mengenal yang namanya **VirtualBox**. Kira-kira hampir mirip lah, bedanya kalo libvirt itu hanya menyediakan Virtualization API saja.

Kalo VirtualBox dia memiliki GUI yang dapat kita klak-klik untuk membuat sebuah VM, di libvirt saya menggunakan `virt-manager` untuk GUI-nya dan `virsh` untuk CLI-nya. Jadi, alasan saya menulis ini karena si Vagrant sudah mendukung libvirt sebagai provider-nya untuk membuat VM, dan saya yakin belum ada orang yang menulis panduan instalasi untuk [openSUSE Leap 15.2](https://opensuse.id) dalam bahasa Indonesia (meskipun saya belum sempat googling untuk membuktikan keyakinan saya yang satu ini).


## Proses instalasi

Seperti tertulis di judul, saya menggunakan sistem operasi openSUSE Leap 15.2 di laptop saya sebagai kendaraan kerja sehari-hari (*daily driver*). Jadi, perintah-perintah berikut akan relevan dijalankan pada sistem operasi tersebut. Jika pembaca menggunakan sistem operasi lain dan hanya membutuhkan referensi pada catatan ini, silakan melanjutkan membaca atau langsung menuju pada bagian referensi di bawah.


{: .box-error}
**Catatan:** Jangan copas perintah apapun tanpa memahami fungsi dari perintah tersebut.

Pertama, kita install `libvirt` beserta `virt-manager` sebagai komponen utama pada tutorial ini. Meskipun sebenarnya paket `virt-manager` ini opsional, tetap saya pasang karena saya membutuhkannya sebagai antar muka grafisnya.

```
sudo zypper install virt-manager qemu libvirt qemu-kvm
```

Kira-kira seperti ini nih penampakan grafisnya di laptop saya:

![Tampilan virt-manager atau Virtual Machine Manager](https://i.imgur.com/lAQKf0K.png){: .mx-auto.d-block :}

Oke, selanjutnya kita pasang `vagrant` beserta `vagrant-libvirt`, yaitu plugin libvirt untuk vagrant. Ini penting agar vagrant dapat terhubung ke libvirt.

```
sudo zypper install vagrant vagrant-libvirt vagrant-vim
```

{: .box-warning}
**Catatan:** Perhatikan bahwa perintah-perintah pada catatan ini dijalankan dengan user biasa. Jika pembaca mengikuti catatan ini dan terlanjur menggunakan `root`, silakan ubah variabel `$USER` di bawah dengan user yang dimaksud (user biasa).

```
sudo usermod -aG libvirt $USER
```

Perintah tersebut berfungsi untuk menambahkan `user` biasa ke dalam grup `libvirt`. Lakukan verifikasi dengan perintah berikut:

```
grep libvirt /etc/group
```

Jika hasilnya sebagai berikut (perhatikan ada penambahan user `samsul` atau user yang dimaksud di akhir baris):

```
libvirt:x:453:samsul
```

maka perintah tadi berhasil. Selanjutnya kita edit file konfigurasi libvirt sebagai berikut:


```
sudo vim /etc/libvirt/libvirtd.conf
```

Hapus komentar pada baris-baris yang berisi teks berikut:

```
unix_sock_group = "libvirt"
unix_sock_ro_perms = "0777"
unix_sock_rw_perms = "0770"
unix_sock_admin_perms = "0700"
```

Simpan dan keluar dengan menekan tombol `Escape`, lalu `:wq`. Selanjutnya agar perubahan tadi dapat berpengaruh ke sistem, lalukan restart service `libvirtd` sebagai berikut.


```
sudo systemctl restart libvirtd
sudo systemctl status libvirtd
```

Sampai di sini, vagrant dengan provider libvirt siap digunakan. Langkah berikutnya saatnya kita menggunakan vagrant. Pastikan perintah `vagrant` dapat dieksekusi di terminal kita.

```
vagrant --version
```

Lalu kita siapkan direktori project-nya, misalnya saya akan membuat seperti berikut :

```
mkdir project1
cd project1
vagrant init centos/8 --box-version 1905.1
```

Perintah-perintah tersebut secara berurutan, kita membuat sebuah folder bernama `project1`, pindah ke dalam folder tersebut, lalu kita inisiasi project vagrant dengan perintah `vagrant init`. Perintah terakhir akan membuat sebuah file bernama `Vagrantfile`, di dalam file tersebut dideskripsikan tipe mesin, OS, dan versi yang akan digunakan. Referensi lengkap tentang Vagrantfile dapat dibaca [di sini](https://www.vagrantup.com/docs/vagrantfile).

Untuk menjalankannya kita eksekusi perintah berikut :

```
vagrant up --provider=libvirt
```

Untuk lebih jelasnya saya buatkan screencast sebagai berikut:

[![asciicast](https://asciinema.org/a/3AXE85WQuXfjSkhsRjoOgJycK.svg)](https://asciinema.org/a/3AXE85WQuXfjSkhsRjoOgJycK)

VM yang dibuat oleh vagrant juga dapat dilihat/dimodifikasi/diapain aja melalui Virt Manager :

![Menampilkan VM yang dibuat oleh Vagrant](https://i.imgur.com/VBNSSrF.png){: .mx-auto.d-block :}


## Perintah-perintah Vagrant

Daftar perintah berikut merupakan perintah yang sering digunakan, khususnya saya sendiri dalam pekerjaan sehari-hari. Untuk merujuk daftar yang lebih lengkap dapat dilihat dengan perintah `vagrant --help` atau merujuk langsung ke [dokumentasinya](https://www.vagrantup.com/docs/cli).

- **up**

Perintah ini akan membuat dan menjalankan mesin guest berdasarkan file `Vagrantfile` yang dibuat. Ini merupakan perintah yang paling penting, karena digunakan untuk membuat mesin Vagrant. Siapa pun yang menggunakan Vagrant pastinya akan menggunakan perintah ini untuk pekerjaan sehari-harinya.

- **ssh**

Perintah ssh digunakan untuk masuk ke dalam VM yang dibuat oleh Vagrant. Ketika pertama kali mesin Vagrant dibuat dengan perintah `vagrant up`, secara otomatis akan dibuatkan key-pair yang selanjutnya akan digunakan oleh perintah `ssh` untuk SSH ke dalam mesin. Seperti dapat Anda lihat pada screencast tadi.

- **halt**

Saat jam kerja sudah usai, sedangkan pekerjaan Anda belum selesai dan akan dilanjutkan pada hari berikutnya, Anda dapat mematikan mesin Vagrant tanpa menghapusnya dengan perintah ini `vagrant halt`.

- **provision**

Menjalankan provisioner yang sudah didefinisikan di dalam `Vagrantfile`, misalnya di dalam file tersebut kita tambahkan script untuk menginstall docker. Agar docker secara otomatis terinstall tanpa kita lakukan ssh ke dalam VM.

- **reload**

Perintah `vagrant reload` jika file `Vagrantfile` dimodifikasi saat mesin Vagrant sedang berjalan. Dengan menggunakan `reload` perubahan yang dibuat akan diterapkan pada mesin.

- **status**

Untuk melihat apakah mesin pada proyek saat ini sedang berjalan atau mati.

Itu saja daftar perintah yang saya jelaskan di sini. Sekali lagi, untuk melihat penjelasan yang lebih detail dapat merujuk langsung ke [dokumentasinya resminya](https://www.vagrantup.com/docs/cli).


Ingin belajar lebih jauh tentang teknologi Cloud? atau ingin menyewa jasa kami? boleh lah intip ke sini --> [https://nacita.id](https://nacita.id/layanan/jasa/)

## Referensi
- [https://github.com/vagrant-libvirt](https://github.com/vagrant-libvirt/vagrant-libvirt/)
- [https://libvirt.org](https://libvirt.org/index.html)
- [https://www.vagrantup.com/docs/index](https://www.vagrantup.com/docs/index)
