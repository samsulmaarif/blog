---
layout: post
title: Ngoprek FreeDOS
date: '2012-03-14T10:42:00.000+07:00'
author: Samsul Maarif
categories: blog
tags:
- Tutorial
- freedos
modified_time: '2012-04-30T19:23:18.783+07:00'
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-1899169524647644028
blogger_orig_url: http://blog.samsul.web.id/2012/02/ngoprek-freedos.html
---

Berawal dari tugas yang diberikan dosen, saya mencoba membuat, sebenarnya bukan membuat, tapi cuma sekedar ngoprek, membuat liveCD DOS.  
Tugasnya memang membuat liveCD - DOS, tapi saya membuatnya dengan FreeDOS. Saya menggunakan Linux Mint 12 KDE (lisa), Berikut ini adalah langkah-langkahnya :

download dulu FreeDOS di [http://www.freedos.org](http://www.freedos.org/)  
Karena ternyata FreeDOS ini hanya berupa installer, bukan liveCD maka perlu dioprek untuk menjadi sebuah liveCD sekaligus instaler. Nama filenya [fd11src.iso](http://www.freedos.org/freedos/files/download/fd11src.iso)  
download image freeDOS yang dapat diboot secara live di [http://www.finnix.org/Balder](http://www.finnix.org/Balder)  
image inilah yang akan diload saat menjalankan liveCD. Nama filenya [balder10.img](http://www.finnix.org/files/balder10.img)

buat folder untuk memount file fd11src.iso

$ sudo mkdir -p ~/liveCD/fdos

mount file iso tadi ke folder tersebut

$ sudo mount fd11src.iso -o loop ~/liveCD/fdos

salin isinya ke folder yang baru agar dapat dioprek

$ mkdir ~/liveCD/custom  
$ rsync -avH ~/liveCD/fdos ~/liveCD/custom

masuk ke folder custom

$ cd ~/liveCD/custom

salin file balder10.img yang kita download tadi ke dalam folder ini

$ cp ~/Unduhan/balder10.img ~/liveCD/custom

ganti namanya menjadi balder.img saja (ini opsional, namun ini akan memengaruhi opsi selanjutnya)

$ mv balder10.img balder.img

hapus file yang tidak diperlukan

$ rm autorun.inf setup.bat

masuk ke folder isolinux untuk mengedit file isolinux.cfg

$ cd isolinux  
$ vim isolinux.cfg

rubah isi filenya agar menjadi seperti ini :

> > UI /isolinux/menu.c32
> 
> > DEFAULT freedos
> 
> > PROMPT 2
> 
> > TIMEOUT 500
> 
> > MENU TITLE Welcome to FreeDOS 1.1 [http://www.freedos.org]
> 
> > MENU IMMEDIATE
> 
> > label freedos
> 
> > menu label ^Jalankan FreeDOS
> 
> > menu default
> 
> > text help
> 
> > Jalankan FreeDOS tanpa memberikan efek apapun pada komputer Anda
> 
> > endtext
> 
> > linux /isolinux/memdisk
> 
> > initrd /balder.img
> 
> > menu separator
> 
> > label freedos
> 
> > menu label ^Install to harddisk
> 
> > menu default
> 
> > text help
> 
> > Install the FreeDOS 1.1 operating system from CD-ROM (or image file) to harddisk
> 
> > The SETUP process is able to format drive/volume C:, copy/unpack files, as well
> 
> > as generate the necessary code and configuration files to startup from harddisk.
> 
> > endtext
> 
> > linux /isolinux/memdisk
> 
> > initrd /isolinux/fdboot.img
> 
> > label h
> 
> > menu label Boot from system ^harddisk
> 
> > text help
> 
> > Jalankan sistem operasi yang telah terinstall di harddisk komputer kamu
> 
> > endtext
> 
> > localboot 0x80
> 
> > label a
> 
> > menu label Boot from ^diskette
> 
> > text help
> 
> > Jalankan dari sistem bootdisk jika Anda masih memiliki sebuah floppydrive
> 
> > dan diskette
> 
> > endtext
> 
> > localboot 0x00

finally buat isonya dengan perintah berikut :

$ mkisofs -r -V "FreeDOS-Live-Samsul" \   
-o freedos-live.iso -b ~/liveCD/custom/isolinux/isolinux.bin \  
-c ~/liveCD/custom/isolinux/boot.cat -cache-inode \  
-J -l -no-emul-boot -boot-load-size 4 -boot-info-table \  
~/liveCD/custom/

berikut output dari perintah tersebut :

I: -input-charset not specified, using utf-8 (detected in locale settings)  
Size of boot image is 4 sectors -> No emulation  
 23.99% done, estimate finish Mon Feb 13 17:14:15 2012  
 47.94% done, estimate finish Mon Feb 13 17:14:15 2012  
 71.93% done, estimate finish Mon Feb 13 17:14:13 2012  
 95.91% done, estimate finish Mon Feb 13 17:14:16 2012  
Total translation table size: 2048  
Total rockridge attributes bytes: 54669  
Total directory bytes: 137216  
Path table size(bytes): 520  
Max brk space used 82000  
20861 extents written (40 MB)

prosesnya tidak memakan banyak waktu, karena filenya kecil.

kini tinggal diujicoba dengan mesin virtual, saya menggunakan QEMU :

$ qemu -cdrom freedos-live.iso

**  
**

**Resource :**

*   [http://www.finnix.org/Balder](http://www.finnix.org/Balder)
*   [http://sourceforge.net/apps/mediawiki/freedos/index.php?title=Main_Page](http://sourceforge.net/apps/mediawiki/freedos/index.php?title=Main_Page)
*   [http://www.freedos.org](http://www.freedos.org/)
*   [http://www.syslinux.org/wiki/index.php/ISOLINUX](http://www.syslinux.org/wiki/index.php/ISOLINUX)
