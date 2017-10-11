---
layout: post
title: Git Version Control System
date: '2017-06-14'
author: Samsul Maarif
categories: blog
tags:
- Linux
- Git
- VCS
bigimg: 
  - "/img/dbs-episode-80-an.jpg" : "Dragonball Super (2017)"
share-img: "/img/dbs-episode-80-an.jpg"
---

Halo pembaca, kali ini saya akan menuliskan sekilat tentang Version Control System (VCS) favorit saya; Git. Ya, Git adalah salah satu dari VCS yang banyak digunakan oleh para developer. Git itu sendiri dibuat oleh orang yang sama pembuat Kernel Linux. Yup, beliau adalah pak [Linus Torvalds](https://plus.google.com/+LinusTorvalds). 

## Tentang Version Control

Sebelum membahas apa itu VCS, mari simak beberapa gambar berikut. 

|![](/img/lowongan-git.jpg)|
|Iklan lowongan pekerjaan [Git](https://www.indeed.com/q-Git-jobs.html)|

Ada juga lowongan yang baru-baru ini di milis \[id-python\]:

|![](/img/lowongan-python-id-git.jpg)|
|Lowongan di milis Python Indonesia|

Oke, baiklah. Cukup menarik, bukan? Tidak sedikit pekerjaan yang mengharuskan atau mensyaratkan penguasaan VCS. Nah, dalam hal ini yang akan kita pelajari adalah Git.

> Version control adalah sebuah sistem yang merekam perubahan-perubahan dari sebuah berkas atau sekumpulan berkas dari waktu-ke waktu sehingga Anda dapat menilik kemmbali versi khusus suatu saat nanti. 

Jika Anda adalah seorang perancang grafis atau web dan ingin menyimpan setiap versi dari sebuah gambar atau layout (yang tentunya Anda ingin melakukannya), sebuah Version Control System (VCS) adalah hal yang bijak untuk digunakan. VCS memperbolehkan Anda untuk mengembalikan berkas-berkas ke keadaan sebelumnya, mengembalikan seluruh proyek kembali ke keadaan sebelumnya, membandingkan perubahan-perubahan di setiap waktu, melihat siapa yang terakhir mengubah sesuatu yang mungkin menimbulkan masalah, siapa dan kapan yang mengenalkan sebuah isu dan banyak lagi. Menggunakan VCS secara umum juga berarti bahwa jika Anda melakukan kesalahan atau menghilangkan berkas, Anda dapat dengan mudah memulihkannya. Sebagai tambahan, Anda mendapatkan semua ini dengan biaya yang sangat sedikit.

Oke, kira-kita begitulah kawan-kawan. Nah, VCS itu ada bermacam-macam, antara lain yang pernah saya sebutkan di [postingan lalu](http://blog.samsul.web.id/2017/06/konversi-vcs-bazaar-ke-git.html) antara lain [Bazaar](http://bazaar.canonical.com/), [Git](http://git-scm.com),  [Mercurial](https://www.mercurial-scm.org/), [Subversion](https://subversion.apache.org/), dll. Kenapa saya memilih Git? Jawabannya ada [di bawah ini](#sejarah-singkat-git).

## Terpusat vs Terdistribusi

Banyak orang mungkin melakukan pengontrolan versi dengan menyalin berkas-berkas pada direktori yang berbeda. Cara ini sangat sederhana, namun cenderung rentan terhadap kesalahan. Oleh karena itulah sistem database versi diciptakan.

|![](/img/git-basisdata-versi.png)|
|Version Control Lokal|

Kelemahan dari sistem ini adalah pengembang perlu melakukan kolaborasi dengan pengembang pada sistem lainnya. Untuk mengatasi permasalahan ini maka dibangunlah Centralized Version Control System (CVCSs).

|![](/img/git-version-control-terpusat.png)|
|Version Control Terpusat|

Sistem seperti ini memiliki beberapa kelebihan, terutama jika dibandingkan dengan VCS lokal. Misalnya, setiap orang pada tingkat tertentu mengetahui apa yang orang lain lakukan pada proyek. Administrator memiliki kendali yang mantap atas siapa yang dapat melakukan apa; dan adalah jauh lebih mudah untuk mengelola sebuah CVCS dibandingkan menangani database lokal pada setiap client.

Walau demikian, sistem dengan tatanan seperti ini memiliki kelemahan serius. Kelemahan nyata yang direpresesntasikan oleh sistem dengan server terpusat. Jika server mati untuk beberapa jam, maka tidak ada seorangpun yang bisa berkolaborasi atau menyimpan perubahan terhadap apa yang mereka telah kerjakan.

Jika harddisk yang menyimpan basisdata mengalami kerusakan, dan salinan yang beran belum tersimpan, anda akan kehilangan setiap perubahan dari proyek kecuali snapshot yang dimiliki oleh setiap kolaborator pada komputernya masing-masing. VCS lokal juga mengalami nasib yang sama jika anda menyimpan seluruh history perubahan proyek pada satu tempat, anda mempunyai resiko kehilangan semuanya. 


|![](/img/git-version-control-terdistribusi.png)|
|Version Control Terdistribusi|

Lain halnya dengan DVCS (Distributed Version Control System) seperti Git, Mercurial, Bazaar atau Darcs. Klien tidak hanya melakukan checkout untuk snapshot terakhir setiap berkas, namun klien memiliki salinan penuh dari repositori tersebut. 

Jadi, jika server mati, dan sistem berkolaborasi melalui server tersebut, maka klien manapun dapat mengirimkan salinan repositori tersebut kembali ke server. Setiap checkout pada DVCS merupakan sebuah backup dari keseluruhan data.

## Sejarah Singkat Git

Dari yang pernah saya baca, Git dikembangkan karena kebutuhan pengembangan kernel Linux. Sebelum menggunakan Git, pengembangan kernel Linux menggunakan DVCS (Distributed Version Control System) proprietary yang bernama BitKeeper, itu dimulai pada tahun 2002 hingga 2005.

Pada tahun 2005, hubungan antara komunitas pengembang Kernel Linux dan perusahaan komersil pembuat BitKeeper menjadi kurang baik, membuat status **free-of-charge** dicabut. Ini membuat komunitas pengembang Linux (dan terutama Linus Torvalds, pembuat Linux) akhirnya mengembangkan sendiri peralatan berdasarkan beberapa halyang dipelajari saat menggunakan BitKeeper. Beberapa tujuan sistem baru tersebut dibuat antara lain :

- Kecepatan
- Desain sederhana
- Dukungan penuh untuk pengembangan non-linear (ribuan cabang secara paralel)
- Terdistribusi secara penuh
- Dapat menangani proyek skala besar seperti Kernel Linux secara efisien (kecepatan dan ukuran data)

## Menginstall Git

Menginstall git cukup mudah, terutama ketika kita pake Distro Linux yang populer. Kita hanya perlu menginstall sebuah paket bernama `git`. 

- Di Debian dan turunannya, kita dapat menginstall dengan perintah :

```
$ sudo apt-get install git
```

- Di CentOS dan turunannya, gunakan perintah :

```
$ sudo yum install git
```

- Di Fedora dan turunannya, dapat menggunakan **dnf** :

```
$ sudo dnf install git
```

Di sistem operasi lain, silakan dicari dan dicoba sendiri.

## Memulai Git Pertama Kali

Setelah menginstall **Git** pada sistem, kita harus melakukan beberapa hal sebelum Git dapat sepenuhnya digunakan. 

- Atur identitas Anda

```
$ git config --global user.name "Samsul Ma'arif"
$ git config --global user.email samsul@puskomedia.id
```

- Atur editor yang akan digunakan

```
$ git config --global core.editor vim
```

- Perkakas diff

```
$ git config --global merge.tool vimdiff
```

Perintah-perintah tersebut akan menghasilkan sebuah berkas bernama `~/.gitconfig` yang isinya sebagai berikut

```
$ cat .gitconfig 
[user]
	name = Samsul Ma'arif
	email = samsul@puskomedia.id
[core]
	editor = vim
[merge]
	tool = vimdiff
```

Dapat pula kita konfirmasikan/cek hasil konfigurasi kita dengan perintah :

```
$ git config --list
user.name=Samsul Ma'arif
user.email=samsul@puskomedia.id
core.editor=vim
merge.tool=vimdiff
```

Oke, tahap persiapan sudah cukup. Mari lanjut ke tahap penggunaan Git.

## Git Basic - Membuat repository

Anda dapat membuat proyek Git dengan dua pendekatan. Pertama dengan direktori proyek yang telah ada di komputer lokal Anda atau [mengimpornya ke Git](http://blog.samsul.web.id/2017/06/konversi-vcs-bazaar-ke-git.html). Kedua dengan mengunduh (clone) dari repositori Git di server yang sudah ada. 

### Inisiasi proyek dari sebuah direktori

Jika Anda memulai merekam sebuah direktori proyek yang sudah ada dengan Git, masuk ke direktori tersebut lalu jalankan `git init` :

```
$ cd ~
$ mkdir my_project
$ cd my_project
$ git init
Initialized empty Git repository in /home/samsul/my_project/.git/
```

Seperti terlihat di sana, perintah tersebut akan membuat sebuah sub-direktori bernama `.git`. Direktori ini berisi semua berkas yang diperlukan oleh sebuah repositori. Pada tahap ini, belum ada yang tersimpan/terekam oleh Git pada repositori kita. 

Jika Anda ingin memulai untuk merekam perubahan-perubahan pada direktori tersebut, Anda perlu mengetikkan beberapa perintah antara lain `git add` dan `git commit` untuk menyimpan perubahan.

```
$ touch README.md
$ git add README.md
$ git commit -m "commit pertama"
```

Sampai di sini, kita telah memiliki sebuah repositori dan telah merekam sebuah perubahan berupa berkas baru bernama `README.md`.

### Mengunduh/cloning Repositori yang sudah ada

Jika Anda ingin menyalin sebuah repositori, misalnya Anda ingin berkontribusi pada sebuah proyek opensource, Anda dapat melakukannya dengan perintah `git clone`. Skema perintahnya adalah `git clone [url]`, di mana \[url\] merupakan alamat repositori di server.

```
$ cd ~
$ git clone https://github.com/geany/geany
```

Perintah tersebut akan membuat sebuah direktori `geany` dan sub-direktori `.git` di dalamnya, menarik seluruh data untuk repositori tersebut, dan membuat salinan versi terkini dari repositori tersebut. 

Anda juga dapat melakukan **clone** dengan nama direktori yang berbeda. Caranya dengan menambahkan nama direktori baru di belakang \[url\] :

```
$ git clone https://github.com/geany/geany geany-ku
```

Git memiliki beberapa protokol pengiriman yang dapat digunakan. Pada contoh tersebut kita menggunakan protokol **`https://`**, namun Anda dapat menggunakan **`git://`** atau **`user@server:lokasi/ke/repo.git`**, yang mana menggunakan protokol transfer SSH.

## Merekam perubahan ke Repositori

Setiap berkas dalam direktori kerja **Git** memiliki dua kondisi: **tracked** dan **untraced**. Tracked (berkas terpantau) adalah berkas yang sebelumnya berada di snapshot terakhir; mereka dapat berada dalam kondisi belum terubah, terubah, ataupun staged (berada di area stage). Berkas untracked (tak-terpantau) adalah kebalikannya - merupakan berkas-berkas di dalam direktori kerja yang tidak berada di dalam snapshot terakhir dan juga tidak berada di area staging. Ketika Anda pertama kali menduplikasi sebuah repositori, semua berkas Anda akan terpantau dan belum terubah karena Anda baru saja melakukan checkout dan belum mengubah apapun.

|![](/img/git-lifecycle.png)|
|Siklus status berkas|

Sejalan dengan proses penyuntingan yang Anda lakukan terhadap berkas-berkas tersebut, Git mencatatnya sebagai terubah, karena Anda telah mengubahnya sejak terakhir commit. Anda kemudian memasukkan berkas-berkas terubah ini ke dalam area stage untuk kemudian dilakukan commit, dan terus siklus ini berulang. 

**Bersambung**

## Referensi

- <https://git-scm.com/book/en/v2/>
- <https://multiplestates.wordpress.com/2015/02/05/rename-a-local-and-remote-branch-in-git/>
- 
