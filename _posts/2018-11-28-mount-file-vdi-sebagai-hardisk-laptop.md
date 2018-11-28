---
layout: post
title: Mount File .vdi Sebagai Hardisk Laptop
date: '2018-11-28'
author: Samsul Maarif
categories: blog
tags:
- Hardisk
- VirtualBox
- VDI
- Linux
bigimg: 
  - "/img/ipin-junior-sysadmin-2018-11-27.jpg" : "Ipin si Junior SysAdmin, Kota Malang (2018)"
share-img: "/img/ipin-junior-sysadmin-2018-11-27.jpg"
---

Ini catatan singkat saja, jadi saya ingin me-mount sebuah file **\*.vdi** sebagai hardisk di laptop. Apakah ini perlu? tidak untuk banyak kasus. Namun kali ini saya melakukannya karena saya tidak ingin menyalakan virtualbox. 

Jadi saya mendapatkan sebuah file berformat \*.vdi. File tersebut, dari nama ekstensinya **Virtual Disk Image** merupakan file hardisk untuk aplikasi VirtualBox. Jika kita membuat sebuah VM di VirtualBox, kita harus membuat file sebagai hardisk virtual. Ada beberapa format yang dapat kita gunakan, antara lain VDI (Virtual Disk Image), VHD (Virtual Hard Disk; klas Microsoft Hyper-V), VMDK (Virtual Machine Disk; khas vMWare), dll. Pembahasan kali ini hanya terkait dengan VDI.

> Oiya, OS yang saya gunakan adalan Ubuntu 16.04, varian lain mungkin dapat juga menggunakan cara-cara ini.

Langkah pertama install `qemu` :

```
 sudo apt-get install qemu
```

Selanjutnya load kernel module *network block device* :

```
 sudo rmmod nbd
 sudo modprobe nbd max_part=16
```

Selanjutnya lakukan proses binding file \*.vdi ke salah satu nbd yang baru saja kita buat, misalnya dalam hal ini file VDI yang saya punya bernama *MoodleCBT_v35_r201811.vdi*

```
 sudo qemu-nbd -c /dev/nbd0 MoodleCBT_v35_r201811.vdi
```

Selanjutnya paksa kernel untuk membaca block device yang baru dengan perintah :

```
 sudo partprobe
```

Cek hasilnya dengan perintah `sudo fdisk -l` outputnya kira-kira akan begini di akhir baris :

```
Disk /dev/nbd0: 30 GiB, 32212254720 bytes, 62914560 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xacdbd653

Perangkat   Boot    Start    Akhir   Sektor Size Id Tipe
/dev/nbd0p1 *        2048 54527999 54525952  26G 83 Linux
/dev/nbd0p2      54530046 62912511  8382466   4G  5 Extended
/dev/nbd0p5      54530048 62912511  8382464   4G 82 Linux swap / Solaris

```


Sekarang kita dapat melakukan mount partisinya ke folder di laptop kita :

```
 sudo mkdir /mnt/folder
 sudo mount /dev/nbd0p1 /mnt/folder
```


Tadaa... Sekarand kita dapat melihat/copy isi-isi folder/file apapun yang ada di dalam virtual disk tersebut. Kita juga dapat melakukan *chroot* ke direktori tersebut. 

```
 sudo chroot /mnt/folder
```

Untuk keluar dari lingkungan *chroot* ketik `exit` atau tekan **Ctrl+D**. Nah, jika kita sudah selesai ngapain aja di sana, unmount file system tersebut dan matikan layanan *qemu-nbd* 


```
 sudo unmount /mnt/folder
 sudo qemu-nbd -d /dev/nbd0
```


Sudah, itu dulu ya catatan saya. Semoga bermanfaat.