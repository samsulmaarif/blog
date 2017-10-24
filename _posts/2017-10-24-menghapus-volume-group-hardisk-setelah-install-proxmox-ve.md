---
layout: post
title: Menghapus Volume Group Hardisk Setelah Install Proxmox VE
date: '2017-10-24'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Virtualisasi
- Proxmox
- LVM
- Hardisk
bigimg: 
  - "/img/family-time_Muktisari_20170215.jpg" : "Family Time, Muktisari, Cilacap (2017)"
share-img: "/img/family-time_Muktisari_20170215.jpg"
---

Nah, ini adalah catatan untuk mengingat pengalaman yang pernah saya alami. Diambil dari berkas log yang sempat saya simpan di laptop saya. Saat itu saya masih mengajar di SMK TI Kawunganten, Cilacap. Setelah praktik install proxmox VE, hardisk tersebut akan digunakan untuk keperluan lain. E, tapi, ternyata hardisk tersebut tidak dapat diformat. 

Seperti kita ketahui, Proxmox VE menggunakan model partisi LVM (Logical Volume Management). Sebuah sistem untuk mengelola volume atau sistem berkas secara logikal. Lebih fleksibel dan canggih daripada metode tradisional pemartisian sebuah cakram dalam satu atau beberapa segmen, dan memformat partisi tersebut dengan sistem berkas. Selengkapnya tentang LVM dapat dibaca di [wikipedia.org](https://en.wikipedia.org/wiki/Logical_volume_management).

Tidak benar-benar ingat kata kunci apa yang saya gunakan saat itu. Namun beginilah kira-kira yang saya lakukan untuk menghapus Volume Group partisi LVM di hardisk tersebut. Oya, proses ini saya lakukan dengan booting live sistem Linux Mint. Jadi, siapkan/buat USB Bootable Linux Mint, bisa pake apa aja, [Multisystem](http://liveusb.info/multisystem), `dd`, [unetbootin](https://www.google.co.id/search?q=unetbootin), dll. Yang penting itu Flashdisk bisa booting live Distro Linux apa pun.

Setelah berhasil membuat live USB, tancapkan Flashdisk tadi ke komputer yang tertanam hardisk bekas instalasi Proxmox. Nyalakan komputer, masuk ke BIOS, atur boot order agar booting melalui Flashdisk. 

Dalam mode live, di desktop Linux, bukalah terminal emulator, lalu ketikkan perintah berikut:

```sh
$ sudo -i
```

Perintah tersebut untuk masuk ke mode super-user. Jika berhasil shell prompt akan berubah menjadi tagar alias tanda pagar (#). Selanjutnya, ketikkan perintah `vgdisplay` untuk menampilkan **Volume Group** yang ada di hardisk. Outputnya kira-kira begini:

```
mint mint # vgdisplay
  --- Volume group ---
  VG Name               pve
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  21
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                5
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               931.50 GiB
  PE Size               4.00 MiB
  Total PE              238465
  Alloc PE / Size       207991 / 812.46 GiB
  Free  PE / Size       30474 / 119.04 GiB
  VG UUID               zGX2Ky-0k9C-9hBz-1OVd-Kq3C-R2ht-C2OP8Y
```

Nah, terlihat di situ, nama Volume Group-nya adalah `pve`. Hapus VG tersebut dengan perintah `vgremove pve`, misapnya seperti ini:

```
mint mint # vgremove pve
Do you really want to remove volume group "pve" containing 5 logical volumes? [y/n]: y
Removing pool data will also remove 4 thin volume(s). OK? [y/n]: y
Do you really want to remove and DISCARD logical volume vm-200-disk-1? [y/n]: y
  /usr/sbin/thin_check: execvp failed: No such file or directory
  Check of thin pool pve/data failed (status:2). Manual repair required (thin_dump --repair /dev/mapper/pve-data_tmeta)!
  Failed to update thin pool data.
```

Ups, sepertinya ada yang galat. Disitu diminta untuk mengetikkan `thin_dump --repair /dev/mapper/pve-data_tmeta` untuk memperbaiki yang rusak. 

```
mint mint # thin_dump --repair /dev/mapper/pve-data_tmeta
The program 'thin_dump' is currently not installed. You can install it by typing:
apt-get install thin-provisioning-tools
```

Ups lagi, perlu menginstal paketnya dulu. Untuk proses instalasi ini memerlukan koneksi internet. Jadi sambungkan dulu desktop Linux Anda ke Internet. Lalu instal paket yang diminta:

```
mint mint # apt-get install thin-provisioning-tools
```

Pastikan tidak ada galat (error/kesalahan) saat install paket tersebut. Kalau gagal bagaimana? Ya coba lagi, bisa jadi yang bermasalah koneksinya, atau hal lain, pokoknya sampe berhasil. Hehehehe.... 

Jalankan kembali perintah tadi:

```
mint mint # thin_dump --repair /dev/mapper/pve-data_tmeta
<superblock uuid="" time="0" transaction="5" data_block_size="1024" nr_data_blocks="1663512">
  <device dev_id="1" mapped_blocks="5686" transaction="0" creation_time="0" snap_time="0">
    <single_mapping origin_block="0" data_block="11817" time="0"/>
    <range_mapping origin_begin="2" data_begin="11833" length="2" time="0"/>
    <range_mapping origin_begin="4" data_begin="15276" length="4" time="0"/>
    .........
    ...............(skip)
```

Akan ada ribuan baris yang muncul, apalagi kalau hardisknya besar. Saya skip saja output lognya. 

Proses perbaikan telah selesai, berikutnya eksekusi penghapusan VG. 

```
mint mint # vgremove pve
Do you really want to remove volume group "pve" containing 5 logical volumes? [y/n]: y
Removing pool data will also remove 4 thin volume(s). OK? [y/n]: y
Do you really want to remove and DISCARD logical volume vm-200-disk-1? [y/n]: y
  Logical volume "vm-200-disk-1" successfully removed
Do you really want to remove and DISCARD logical volume vm-110-disk-1? [y/n]: y
  Logical volume "vm-110-disk-1" successfully removed
Do you really want to remove and DISCARD logical volume vm-100-disk-1? [y/n]: y
  Logical volume "vm-100-disk-1" successfully removed
Do you really want to remove and DISCARD logical volume vm-110-disk-2? [y/n]: y
  Logical volume "vm-110-disk-2" successfully removed
Do you really want to remove and DISCARD logical volume data? [y/n]: y
  Logical volume "data" successfully removed
Do you really want to remove and DISCARD logical volume lvol0_pmspare? [y/n]: y
  Volume group "pve" successfully removed
```

Jawab setiap pertanyaan seperti pada log di atas untuk menghapus setiap volume logikal yang ada pada hardisk tersebut. Konfirmasikan apakah Volume Group benar-benar telah dihapus atau belum:

```
mint mint # vgdisplay
  No volume groups found
```

Nah, Volume Group telah berhasil dihapus. Restart komputernya, sekarang sudah dapat digunakan untuk menginstal sistem operasi lain atau untuk keperluan yang lain. Selamat mencoba.







