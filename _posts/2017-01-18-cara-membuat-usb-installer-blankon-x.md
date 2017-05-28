---
layout: post
title: Cara Membuat USB Installer BlankOn X dengan Multisystem
date: '2017-01-18T01:58:00.002+07:00'
author: Samsul Ma'arif
categories: blog
tags:
- Linux
- OpenSource
- Live USB
- BlankOn
- Installer
modified_time: '2017-01-18T02:12:12.476+07:00'
thumbnail: https://3.bp.blogspot.com/-me0A3AC5HXA/WH5pVjveGFI/AAAAAAAAQE0/xWhKPUn-MWUqE-GEmMMrZxI9130MdbtOQCLcB/s72-c/blankon.png
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-7312796844235598279
blogger_orig_url: http://blog.samsul.web.id/2017/01/cara-membuat-usb-installer-blankon-x.html
---

[![](https://3.bp.blogspot.com/-me0A3AC5HXA/WH5pVjveGFI/AAAAAAAAQE0/xWhKPUn-MWUqE-GEmMMrZxI9130MdbtOQCLcB/s320/blankon.png)](https://3.bp.blogspot.com/-me0A3AC5HXA/WH5pVjveGFI/AAAAAAAAQE0/xWhKPUn-MWUqE-GEmMMrZxI9130MdbtOQCLcB/s1600/blankon.png)

Baru-baru ini BlankOn X dengan nama kode [_Tambora_ dirilis](https://www.blankonlinux.or.id/catatanrilis.html) oleh pengembang BlankOn setelah tiga tahun pengembangan. Namun beberapa pengguna ternyata kesulitan membuat USB (baca: Flashdisk) installer BlankOn X ini.  

Sebenarnya ada banyak metode untuk membuatnya. Antara lain, dengan metode **dd**, **unetbootin**, dan **multisystem**. Nah, kali ini saya akan membahas bagaimana cara membuatnya dengan Multisystem.  

Baik, sebelumnya saya akan membahas apa itu Multisystem. Multisystem adalah sebuah aplikasi untuk membuat live USB/installer yang tersedia di sistem operasi Linux, khususnya yang berbasis Debian. Dengan aplikasi ini kita dapat membuat banyak installer dalam 1 Flashdisk, asalkan flashdisknya masih muat.  

1. Langkah pertama, install dulu **multisystem**-nya, caranya sebagai berikut :  

a. Download skrip untuk menginstall multisystem :  

```
wget http://liveusb.info/multisystem/install-depot-multisystem.sh.tar.bz2
```

b. Ekstrak lalu jalankan skripnya :  

```
tar -xvjf install-depot-multisystem.sh.tar.bz2  
sudo bash ./install-depot-multisystem.sh
```
ketikkan password, lalu tekan Enter. Tunggu hingga proses instalasi selesai.  

2. Pastikan Anda telah mengunduh file [ISO BlankOn X](http://cdimage.blankonlinux.or.id/blankon/rilis/10.0/), jika belum, silakan unduh terlebih dahulu.  

3. Jalankan multisystem dengan cara mengetikkan **multisystem** di teminal, atau melalui menu aplikasi > aksesoris > multisystem, seperti nampak pada gambar berikut :  

[![](https://1.bp.blogspot.com/-PxI6lXrcwQg/WH5h_6TFpoI/AAAAAAAAQDk/vnkD_YjK0I8pQsDlp-3k9df3WjLeioeeACLcB/s320/Cuplikan%2BLayar_2017-01-18_01-26-01.png)](https://1.bp.blogspot.com/-PxI6lXrcwQg/WH5h_6TFpoI/AAAAAAAAQDk/vnkD_YjK0I8pQsDlp-3k9df3WjLeioeeACLcB/s1600/Cuplikan%2BLayar_2017-01-18_01-26-01.png)

4. Oiya, pastikan flashdisk yang akan digunakan untuk membuat installer telah tertancap di laptop/komputer kita dan telah dimout di sub directory **/media**. Setelah muncul aplikasi multisystemnya, seperti di bawah ini :  

[![](https://4.bp.blogspot.com/-uwCH7j8MZZc/WH5i1Ui7lJI/AAAAAAAAQDs/s46aRJmV5Zgv_AphbhNMrLBfhDlWzDYggCLcB/s320/Cuplikan%2BLayar_2017-01-18_01-27-59.png)](https://4.bp.blogspot.com/-uwCH7j8MZZc/WH5i1Ui7lJI/AAAAAAAAQDs/s46aRJmV5Zgv_AphbhNMrLBfhDlWzDYggCLcB/s1600/Cuplikan%2BLayar_2017-01-18_01-27-59.png)

5. Pastikan flashdisk kita terbaca dan memiliki ruang yang cukup. Jika sudah, klik tombol konfirmasi di kanan bawah.  

[![](https://2.bp.blogspot.com/-2eIaOmngVv4/WH5jzyHazvI/AAAAAAAAQEA/UcL4WJV4CjwN98dY9cPx8oLc60aL3Yh7QCLcB/s320/Cuplikan%2BLayar_2017-01-18_01-33-25.png)](https://2.bp.blogspot.com/-2eIaOmngVv4/WH5jzyHazvI/AAAAAAAAQEA/UcL4WJV4CjwN98dY9cPx8oLc60aL3Yh7QCLcB/s1600/Cuplikan%2BLayar_2017-01-18_01-33-25.png)

6. Selanjutnya klik pada bagian **pilih .iso atau .img**, lalu cari file [BlankOn-10.0-desktop-amd64.iso](http://cdimage.blankonlinux.or.id/blankon/rilis/10.0/BlankOn-10.0-desktop-amd64.iso) atau [BlankOn-10.0-desktop-i386.iso](http://cdimage.blankonlinux.or.id/blankon/rilis/10.0/BlankOn-10.0-desktop-i386.iso), akan muncul tampilan sebagai berikut :  

[![](https://4.bp.blogspot.com/-XPcPVom6qks/WH5kfqO80WI/AAAAAAAAQEE/uqOGlPGxJ-Ia3Ss3Oy49n0BPiYzzVEDLwCLcB/s320/Cuplikan%2BLayar_2017-01-18_01-37-37.png)](https://4.bp.blogspot.com/-XPcPVom6qks/WH5kfqO80WI/AAAAAAAAQEE/uqOGlPGxJ-Ia3Ss3Oy49n0BPiYzzVEDLwCLcB/s1600/Cuplikan%2BLayar_2017-01-18_01-37-37.png)

ketikkan passwordnya, lalu tekan **Enter**.  

[![](https://3.bp.blogspot.com/-kwHm2WEn_PY/WH5k7OLeLPI/AAAAAAAAQEM/HmAhU_jWPqMiOQVDNOJvQeUiGzBBWDdQQCLcB/s320/Cuplikan%2BLayar_2017-01-18_01-38-47.png)](https://3.bp.blogspot.com/-kwHm2WEn_PY/WH5k7OLeLPI/AAAAAAAAQEM/HmAhU_jWPqMiOQVDNOJvQeUiGzBBWDdQQCLcB/s1600/Cuplikan%2BLayar_2017-01-18_01-38-47.png)

7. Proses ini tidak akan memakan waktu yang cukup lama. Tergantung kekuatan hardware komputer/laptop kita dan kualitas flashdisk yang kita gunakan.  

8. Jika berhasil akan muncul logo BlankOn seperti pada tangkapan layar berikut :  

[![](https://1.bp.blogspot.com/-QZmK9QR98W0/WH5mBL5biqI/AAAAAAAAQEY/n_2AvDhfVDA-Ap107s49gsKCvQ4qMgJ0wCLcB/s320/Cuplikan%2BLayar_2017-01-18_01-43-00.png)](https://1.bp.blogspot.com/-QZmK9QR98W0/WH5mBL5biqI/AAAAAAAAQEY/n_2AvDhfVDA-Ap107s49gsKCvQ4qMgJ0wCLcB/s1600/Cuplikan%2BLayar_2017-01-18_01-43-00.png)

9. Kita dapat menguji hasilnyanya dengan fasilitas yang tersedia di multisystem tersebut, antara lain kita dapat menggunakan **Qemu** atau **VirtualBox**. Atau dengan mencoba langsung di laptop yang akan kita install.  

10. Akan ada menu tambahan baru berupa **BlankOn** di grub multisystem flashdisk, seperti berikut :  

[![](https://1.bp.blogspot.com/-IYRtsOksjew/WH5nXkz0VOI/AAAAAAAAQEo/x6BNUJVt9E8TBw00nCZnkE1vbWGC3j_9gCLcB/s320/Cuplikan%2BLayar_2017-01-18_01-45-04.png)](https://1.bp.blogspot.com/-IYRtsOksjew/WH5nXkz0VOI/AAAAAAAAQEo/x6BNUJVt9E8TBw00nCZnkE1vbWGC3j_9gCLcB/s1600/Cuplikan%2BLayar_2017-01-18_01-45-04.png)

11. Sampai di sini proses pembuatan USB Installer BlankOn X dengan nama kode Tambora telah berhasil.  

## Kesimpulan :

- Pembuatan Live USB maupun installer BlankOn X ini telah dimudahkan dengan menggunakan Multisystem.  
- Semoga catatan ini bermanfaat bagi siapa pun pembacanya,  
- dan mohon doanya, semoga penulis segera menikah tahun ini 👦👧  
- terima kasih.  

## Referensi :  

- [https://samsulmuktisari.wordpress.com/2011/09/29/membuat-liveusb-multiboot-di-blankon/ ](https://samsulmuktisari.wordpress.com/2011/09/29/membuat-liveusb-multiboot-di-blankon/%C2%A0)  
- [https://www.blankonlinux.or.id/catatanrilis.html](https://www.blankonlinux.or.id/catatanrilis.html)  
- [http://distrowatch.com/?newsid=09693](http://distrowatch.com/?newsid=09693)
