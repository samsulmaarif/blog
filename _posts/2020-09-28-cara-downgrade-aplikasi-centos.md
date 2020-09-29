---
layout: post
title: Cara Downgrade Aplikasi di CentOS
date: '2020-09-28'
author: Samsul Maarif
categories: blog
tags:
- Linux
- CentOS
cover-img:
  - "/img/sawah-sidareja.jpg" : "Purwosari, Kedungreja, Cilacap (2019)"
share-img: "/img/sawah-sidareja.jpg"
---

Sebuah aplikasi yang ditulis oleh temen-temen developer terkadang hanya kompatibel dengan versi runtime tertentu pada bahasa pemrogramannya. Hal ini akan menuntut administrator agar dapat menyesuaikan dengan kebutuhan tersebut.

Nah, catatan ini disimulasikan dijalankan di sebuah VM dengan sistem operasi CentOS 8. Catatan ini juga berdasarkan pengalaman penulis saat menangani sebuah proyek di kerjaan. Konfigurasi **Vagrantfile** saya seperti berikut ini :

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/8"
  config.vm.hostname = "nacita-lab1"
  config.vm.provider :libvirt do |domain|
    domain.memory = 2048
    domain.cpus = 2
  end
  config.vm.define "nacita-lab1"
end
```

Jalankan dengan mengetikkan `vagrant up` di terminal, lalu masuk ke vm tersebut dengan `vagrant ssh`.

Untuk temen-temen yang belum pernah menggunakan *Vagrant* boleh baca catatan saya tentang ini **[di sini](/2020/09/menjalankan-vagrant-libvirt-di-opensuse-leap.html)**.

Ceritanya, saya memasang dotnet SDK dengan perintah berikut:

```
sudo rpm -Uvh https://packages.microsoft.com/config/centos/8/packages-microsoft-prod.rpm
sudo yum install dotnet-sdk-3.1
```

Perintah tersebut akan menginstall versi terbaru dari `dotnet`, seperti yang dapat kita lihat dengan perintah seperti ini:

```
[vagrant@nacita-lab1 ~]$ dotnet --list-sdks
3.1.402 [/usr/share/dotnet/sdk]
[vagrant@nacita-lab1 ~]$ dotnet --version
3.1.402
```

Nah, sayangnya pada aplikasi pada project yang sedang dikerjakan tidak kompatibel dengan versi tersebut. Proses build selalu menampilkan pesan error yang saya sudah lupa entah apa, dan setelah saya konfirmasi ke temen-temen developer ternyata di lokal mereka menggunakan versi yang lebih jauh di bawahnya.

Sekarang kita tau apa penyebabnya, yaitu versinya ketinggian. Kita cari tau dulu versi yang tersedia di repo berapa saja.

```
[vagrant@nacita-lab1 ~]$ yum --showduplicates list dotnet-sdk-3.1 | expand
Failed to set locale, defaulting to C
CentOS-8 - AppStream                            143 kB/s | 5.8 MB     00:41    
CentOS-8 - Base                                 405 kB/s | 2.2 MB     00:05    
CentOS-8 - Extras                                10 kB/s | 8.1 kB     00:00    
packages-microsoft-com-prod                     265 kB/s | 493 kB     00:01    
Installed Packages
dotnet-sdk-3.1.x86_64        3.1.402-1              @packages-microsoft-com-prod
Available Packages
dotnet-sdk-3.1.x86_64        3.1.105-1              packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.106-1              packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.107-1              packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.107-1.el8_2        AppStream                   
dotnet-sdk-3.1.x86_64        3.1.108-1              packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.301-1              packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.302-1              packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.401-1              packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.402-1              @packages-microsoft-com-prod
dotnet-sdk-3.1.x86_64        3.1.402-1              packages-microsoft-com-prod
```

Untungnya versi yang kita inginkan, yaitu `3.1.105` tersedia di sana. Jadi, kita bisa eksekusi proses downgrade-nya:

```
[vagrant@nacita-lab1 ~]$ sudo yum downgrade dotnet-sdk-3.1-3.1.105-1
...............................................skip
Downgrade  1 Package

Total download size: 65 M
Is this ok [y/N]: y
...............................................skip

Downgraded:
  dotnet-sdk-3.1-3.1.105-1.x86_64                                                                                                                                            
Complete!
```

Perhatikan penulisan nama paket dan versinya, `dotnet-sdk-3.1` merupakan nama paketnya, disambung dengan tanda hubung (`-`), `3.1.105-1` adalah versi spesifik yang kita inginkan.

Verifikasi hasil downgrade yang telah kita lakukan:

```
[vagrant@nacita-lab1 ~]$ dotnet --list-sdks
3.1.105 [/usr/share/dotnet/sdk]
[vagrant@nacita-lab1 ~]$ dotnet --version
3.1.105
```

Aplikasi telah berhasil kita downgrade (turun versi), dan proyek pun berjalan sesuai yang kita inginkan. Demikian catatan singkat ini, semoga bermanfaat.

{: .box-success }
Ingin belajar lebih jauh tentang server Linux & teknologi *Container*? atau ingin menyewa jasa kami? boleh lah intip ke sini --> [https://nacita.id](https://nacita.id/layanan/jasa/)
