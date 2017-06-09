---
layout: post
title: Konversi VCS Bazaar ke Git
date: '2017-06-01'
author: Samsul Maarif
categories: blog
tags:
- Linux
- VCS
- Bazaar
- Git
bigimg: 
  - "/img/festival-destika-2015.jpg" : "Pantai Tanjung Tinggi, Belitung Timur (2015)"
  - "/img/menjadi-siswa-pak-onno.jpg" : "Menjadi siswa pak Onno W. Purbo di Sekolah Laskar Pelangi (2015)"
share-img: "/img/workshop-server-website-desa.jpg"
---

VCS atau Version Control System adalah sebuah perangkat lunak yang biasa digunakan untuk mencaat setiap perubahan pada kode sumber sebuah aplikasi. Ini merupakan peralatan wajib bagi seorang programmer, atau pengembang aplikasi. Beberapa VCS yang bernah saya coba pakai adalah [Bazaar](http://bazaar.canonical.com/) dan [Git](http://git-scm.com). Lainnya ada [Mercurial](https://www.mercurial-scm.org/), [Subversion](https://subversion.apache.org/), dll selebihnya dapat dibaca di [CodePolitan](https://www.codepolitan.com/10-version-control-system-yang-harus-kamu-kenal) ya.... 

Bazaar merupakan VCS yang disponsori oleh [Cannonical Ltd.](http://www.canonical.com/) yang dipakai untuk pengembangan sistem operasi Ubuntu, lebih spesifiknya untuk mengelola kode sumber aplikasi-aplikasi yang dikelola di [launchpad.net](https://launchpad.net). Bazaar juga digunakan oleh tim pengembang [BlankOn Linux](http://dev.blankonlinux.or.id) untuk mengelola kode sumber untuk pemaketan aplikasi pada lumbungnya. 

Saat ini tim pengembang [BlankOn XI Uluwatu](http://github.com/BlankOn) sedang berupaya memigrasikan lumbungnya ke GitHub dan mengonversikan VCSnya dengan Git. Berikut ini adalah sekelumit catatan proses migrasi/konversi VCS Bazaar ke Git versi saya.

Hal yang pertama perlu dilakukan adalah menginstall paket-paket aplikasi yang diperlukan.

```
$ sudo apt install git-core bzr
```

Buat sebuah berkas `~/.gitconfig` dan isi sesuai nama dan alamat email Anda :

```ini
[user]
        email = hay@samsul.web.id
        name = Samsul Ma'arif

```

Selanjutnya install bzr-fastimport

```bash
$ mkdir -p ~/.bazaar/plugins
$ cd ~/.bazaar/plugins
$ bzr branch lp:bzr-fastimport fastimport
```

Dan saat saya coba, ternyata memerlukan juga modul `python-fastimport`, maka install-lah.

```bash
$ sudo apt install python-fastimport
```

Buat direktori kerja,

```bash
$ mkdir BlankOn
$ cd BlankOn
```

Pilih repository yang akan di clone di : <http://dev.blankonlinux.or.id/browser/>, clone dengan perintah 

```
$ bzr branch alamat_repo_yang_akan_di_clone nama_aplikasi
```

> Perintah `bzr branch` ini mirip/fungsinya sama dengan perintah `git clone` yaitu mengunduh repository ke mesin lokal. 

Misalnya saya akan mengunduh repository `firefox`, maka perintah yang saya jalankan adalah

```bash
$ bzr branch http://dev.blankonlinux.or.id/browser/tambora/firefox firefox
$ cd firefox
```

Inisiasi repository baru dengan **Git** :

```bash
$ git init
```

Lakukan proses migrasi dengan perintah berikut :

```bash
$ bzr fast-export --plain . | git fast-import
```

Perintah tersebut akan memigrasikan seluruh riwayat perubahan yang ada di repository tersebut, yang sebelumnya menggunakan **Bazaar** ke **Git**. Jika terjadi kendala/error akan ditampilkan berikut solusinya. Setelah proses migrasi tersebut, dapat dilihat hasilnya dengan mengetikkan `git log`, maka akan terlihat semua log perubahan yang dibuat dengan **Bazaar** sudah ada dan terkonversi ke **Git**.

Lakukan reset dan hapus direktori `.bzr`, selamat tinggal **Bazaar**.

```bash
$ git reset HEAD
$ rm -rf .bzr/
```

Selanjutnya, buat repository baru di GitHub.com menggunakan browser favorit. Lalu tambahkan alamat repository GitHub ke repo yang baru kita migrasikan tersebut.

```bash
$ git remote add origin https://github.com/blankon-packages/firefox.git
```

Dalam hal ini, saya menambahkan satu perubahan ke repositori tersebut, yaitu menambahkan file `README.md` :

```bash 
$ echo "# firefox blankon package" > README.md
```

Simpan perubahan ke [**Staging area**](#) :

```bash
$ git add -A
```

Simpan perubahan dengan perintah `commit` 

```bash
$ git commit -m "upload to GitHub"
```

Buat cabang baru bernama `tambora`

```bash
$ git checkout -b tambora
```

Unggah ke remote repository (dalam hal ini adalah github.com) dengan perintah `push`

```
$ git push -u origin tambora
$ git push -u origin master
```

Hasilnya dapat dilihat di <https://github.com/blankon-packages/firefox>

|[![](/img/migrasi-bazaar-git.png)](/img/migrasi-bazaar-git.png)|
|Penampakan saat saya push paket `desktop-base`|

## Referensi
- <https://design.canonical.com/2015/01/converting-projects-between-git-and-bazaar/>
