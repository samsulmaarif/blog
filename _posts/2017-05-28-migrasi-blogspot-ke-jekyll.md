---
layout: post
title: Migrasi Blogspot ke Jekyll
date: '2017-05-28'
author: Samsul Maarif
categories: blog
tags:
- Blog
- Migrasi
- Jekyll
- Blogspot
bigimg: 
- "/img/pantai-balekambang_20160724_094830.jpg" : "Pantai Balekambang, Malang, Jawa Timur (2016)"
share-img: "/img/blog-samsul-web-id-lama.png"
thumbnail: "/img/blog-samsul-web-id-lama.png"
---

Blog ini dulunya menggunakan blogspot. Iya, dulunya. Baru beberapa hari ini sih saya migrasikan ke jekyll. Apa sih itu **Jekyll**? apa pula **GitHub Pages**?

Bisa saya tuliskan secara ringkas, bahwa **Jekyll** adalah sebuah mesin blog yang statis yang menggunakan bahasa pemrograman **ruby**. Dalam menulis/ posting tulisan di blog berbasis jekyll, tulisan dibuat dengan format [**Markdown**](https://daringfireball.net/projects/markdown/syntax). 

Lalu apa itu **GitHub Pages**. GitHub sendiri merupakan sebuah website yang menyediakan layanan untuk sharing kode sumber berbagai aplikasi. Biasanya, aplikasi yang ada di GitHub adalah aplikasi FLOSS (Free/Libre Open Source Sofware). Sedangkan **GitHub Pages** adalah salah satu layanan GitHub di mana kita dapat membuat sebuah website/blog secara gratis. Intinya, GitHub Pages itu ibaratnya hostingnya. 

Begini tampilan blog saya sebelum saya migrasikan ke sini :

| ![Tampilan Blog lama dengan blogspot](/img/blog-samsul-web-id-lama.png) |
| Tampilan **blog.samsul.web.id** lama dengan blogspot |

Lalu, bagaimana proses migrasinya? Oke, sabar dulu ya. Kita tuliskan dulu apa syarat-syarat sebelum migrasi dengan cara saya ini.

## Prasyarat
- Syarat pertama adalah, Anda punya blog yang berbasis blogspot, dan di blog itu ada beberapa tulisan yang akan dimigrasikan kontennya. Kalo blognya masih kosong sih, tidak usah migrasi yah. Tinggal bikin blog baru langsung pake **jekyll**. Hehehehe
- Selanjutnya, Anda harus memahami cara menggunakan baris perintah di Linux. Karena dalam catatan ini saya menggunakan Linux, dan semua proses dilakukan dengan sistem operasi GNU/Linux. Btw, sistem operasi yang saya gunakan adalah Ubuntu Studio 16.04 64 bit. Kenapa saya pake Ubuntu Studio? Ya tidak kenapa-napa, saya suka aja pakenya. Jadi, kalau Anda pake sistem operasi lain silakan menyesuaikan sendiri ya.
- Paham cara menggunakan VCS (Version Control System) Git.
- [**Install Jekyll**](http://jekyllrb.com/docs/installation/) di laptop/komputer Anda. Cara yang paling mudah adalah dengan `gem`.


```
$ gem install jekyll jekyll-import
$ gem install bundle
```

## Persiapkan "calon" blog
- Selanjutnya pilih tema yang Anda sukai. Tema jekyll sangat mudah dicari di google dengan satu kata kunci [**jekyll themes**](https://www.google.com/search?client=ubuntu&channel=fs&q=jekyll+themes&ie=utf-8&oe=utf-8), salah satunya yang dapat Anda temukan adalah website [http://themes.jekyllrc.org/](http://themes.jekyllrc.org/). Sekali lagi, pilih tema yang Anda sukai.
- Setelah Anda menentukan tema yang akan digunakan, unduh tema tersebut.


```
$ wget https://github.com/brianmaierjr/long-haul/archive/master.zip
```

- Ekstrak tema tersebut, lalu masuk ke direktori hasil ekstrak tema tersebut. 


```
$ unzip master.zip
$ mv long-haul-master namasaya.github.io
$ cd namasaya.github.io
```

- Lakukan **commit** untuk pertama kalinya.


```
$ git init
$ git add -A
$ git commit -m "Commit pertama" # isi pesannya bebas
```

## Ekspor konten di blogspot
- Lakukan ekspor konten dengan login ke [blogger.com](https://www.blogger.com), 
- pada dashboard di blogger, klik pada **Setelan** > **Lainnya**. 
- Selanjutnya pada bagian **Impor & Backup**, klik **Backup konten**, klik pada **Simpan ke komputer**.
- Simpan berkas tersebut pada direktori kerja yang tadi di buat, dalam hal ini nama foldernya adalah `namasaya.github.io`.

## Impor konten ke jekyll
- Hasil ekspor dari blogger berupa file XML tunggal, di saya namanya `blog-05-27-2017.xml`. File ini yang akan diimpor ke jekyll.
- Namun sebelum melakukan impor, install terlebih dahulu _ketergantungan_ tema yang akan digunakan :


```
$ bundle install
Fetching gem metadata from https://rubygems.org/.............
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Fetching i18n 0.7.0


Your user account isn't allowed to install to the system RubyGems.
  You can cancel this installation and run:

      bundle install --path vendor/bundle

  to install the gems into ./vendor/bundle/, or you can enter your password
  and install the bundled gems to RubyGems using sudo.

  Password: 

```

Ketikkan password `sudo` Anda, lalu tekan **Enter**. Tunggu sampai proses instalasi selesai sambil minum teh (atau kerjakan yang lain).


```
..................
Fetching jemoji 0.7.0
Installing jemoji 0.7.0
Fetching github-pages 106
Installing github-pages 106
Bundle complete! 2 Gemfile dependencies, 57 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
Post-install message from html-pipeline:
-------------------------------------------------
Thank you for installing html-pipeline!
You must bundle Filter gem dependencies.
See html-pipeline README.md for more details.
https://github.com/jch/html-pipeline#dependencies
-------------------------------------------------
Post-install message from minima:

----------------------------------------------
Thank you for installing minima 2.0!

Minima 2.0 comes with a breaking change that
renders '<your-site>/css/main.scss' redundant.
That file is now bundled with this gem as
'<minima>/assets/main.scss'.

More Information:
https://github.com/jekyll/minima#customization
----------------------------------------------
```

- Lakukan impor dengan perintah :


```
$ ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::Blogger.run({
"source" => "/lokasi/blog-05-27-2017.xml", #sesuaikan lokasinya
"no-blogger-info" => false,
"replace-internal-link" => false,
})'

```

- Proses impor ini tidak membutuhkan waktu yang lama. Apalagi jika konten blognya belum banyak.
- Hasil impor akan tersimpan di direktori `_posts` dengan format `YYYY-MM-DD-judul-artikel.html`. Sepertinya ini bukan hasil yang diharapkan oleh jekyll, masih perlu langkah-langkah praktis untuk mengubahnya menjadi format **Markdown**.
- Untuk keperluan tersebut, saya menggunakan layanan dari [https://domchristie.github.io/to-markdown/](https://domchristie.github.io/to-markdown/) untuk mengonversinya dari format **html** menjadi **Markdown**. Tidak praktis memang, karena harus copas satu per satu. Namun sepertinya cukup efektif, karena saya belum menemukan perkakas lainnya yang dapat dilaukan dengan baris perintah.
- Nah, untuk melakukan konversi yang perlu diperhatikan adalah, kita cukup copas bagian kontennya saja. Bagian header yang diawali dan diakhiri tanda `---` dibiarkan sebagaimana adanya. Contoh berikut saya ambilkan dari file `2012-02-30-ngoprek-freedos.html` yang saya konversikan menjadi `2012-02-30-ngoprek-freedos.md` secara manual.


```
---
layout: post
title: Ngoprek FreeDOS
date: '2012-03-14T10:42:00.000+07:00'
author: Samsul Maarif
tags:
- Tutorial
- freedos
modified_time: '2012-04-30T19:23:18.783+07:00'
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-1899169524647644028
blogger_orig_url: http://blog.samsul.web.id/2012/02/ngoprek-freedos.html
---

<p>Ini bagian konten yang akan dikonversi.</p>
```

Begini penampakannya :

|![/img/konversi-html-ke-markdown.png](/img/konversi-html-ke-markdown.png)|
| Konversi Html ke Markdown |


- Salin tempel konten hasil konversi ke tempat asalnya, lalu simpan dengan nama `YYYY-MM-DD-judul-artikel.md`.
- Lakukan pada semua artikel yang dikonversi.
- Selesai? Ternyata belum. Hasil konversi dengan cara tersebut ternyata masih menyisakan beberapa kode **HTML** yang terlewatkan. Seperti misalnya :


```
<span style="color: #38761d;">$ mkdir ~/liveCD/custom</span>  
<span style="color: #38761d;">$ rsync -avH ~/liveCD/fdos ~/liveCD/custom</span>
```

- Hal ini tentu menggangu, dan menghasilkan kode yang tidak bersih. Oleh karena itu, perlu dibersihkan tag-tag HTML yang tidak diperlukan tersebut. Sempat googling sebentar perintah `sed` untuk menghilangkan tag-tag tersebut. 


```
$ sed -e 's/<[^>]*>//g' _posts/*.md
```

- Perintah tersebut artinya hapus seluruh tag HTML (yang biasanya diawali dengan `<tag>` ditutup dengan `</tag>`) dalam direktori `_posts/`, semua file yang berakhiran `.md`. Hanya dalam hitungan detik, perintah ini sukses dijalankan.
- Dengan cara tersebut, hilang pula **embed** video dan embed-embedan yang lain jika suatu postingan terdapat video. Tapi cara ini dapat dengan mudah diatasi dengan cara copas lagi dari file html aslinya.

## Konfigurasi Jekyll
- Kali ini masuk ke bagian konfigurasi Jekyll. Website berbasis jekyll ini ajaib. File konfigurasinya bernama `_config.yml`, silakan langsung intip saja file konfigurasi blog saya [di sini](https://github.com/samsulmaarif/blog/blob/gh-pages/_config.yml){:target="_blank"}. Tidak perlu saya tampilkan di sini karena terlalu panjang.
- Coba jalankan di lokal

```
$ bundle exec jekyll serve --incremental --port=8000

```

- Outputnya kira-kira begini di laptop saya:

```
Configuration file: /lokasi/namasaya.github.io/_config.yml
Configuration file: /lokasi/namasaya.github.io/_config.yml
            Source: /lokasi/namasaya.github.io
       Destination: /lokasi/namasaya.github.io/_site
 Incremental build: enabled
      Generating... 
                    done in 8.016 seconds.
 Auto-regeneration: enabled for '/lokasi/namasaya.github.io'
Configuration file: /lokasi/namasaya.github.io/_config.yml
    Server address: http://127.0.0.1:8000/
  Server running... press ctrl-c to stop.
```

- Tunggu beberapa detik, lalu coba akses melalui browser dengan alamat **http://localhost:8000**. Jika tanpa opsi `--port=8000`, maka port defaultnya adalah **4000**. 
- Jika berhasil akan menampilkan laman web yang baru dibuat. 


|![](/img/tampilan-blog.samsul.web.id.jpg)|
| Tampilan localhost:8000 dengan jekyll |


- Namun jika ada yang error, lihat log errornya. Bisa jadi _ketergantungannya_ ada yang kurang. Jika itu yang terjadi, lakukan update dan install dengan perintah:

```
$ bundle update
$ bundle install
```

Lalu jalankan perintah `serve` lagi.


## Publikasikan dengan GitHub Pages
- Buat akun di [GitHub](https://github.com), jika Anda belum punya. 
- Login, lalu buat repository dengan nama `namasaya.github.io`. Sesuaikan `namasaya` dengan nama akun Anda.
- Kirim perubahan yang ada di lokal ke **GitHub**


```
$ git add -A
$ git commit -m "pesan commit silakan disesuaikan"
$ git remote add origin https://github.com/namasaya/namasaya.github.com
$ git push -u origin master
```

- Masukkan nama pengguna & kata sandi akun GitHub Anda saat diminta.
- Beberapa menit kemudian, website/blog Anda dapat diakses melalui <http://namasaya.github.io>

# Menggunakan Custom Domian
- Anda juga dapat menggunakan domain Anda sendiri untuk dipasangkan pada website/blog di GitHub Pages.
- Sebelumnya, pada panel domain Anda, buat record `CNAME` arahkan ke `namasaya.github.io`. Sekali lagi, sesuaikan `namasaya` dengan nama Akun Anda sendiri.
- Konfigurasi lainnya adalah berkas `CNAME` dalam repository, dalam hal ini saya menggunakan custom domain **blog.samsul.web.id**.


```
$ echo "blog.samsul.web.id" > CNAME
$ git add CNAME
$ git commit -m "menambahkan CNAME"
$ git push origin master
```

- Dengan demikian blog saya dapat diakses melalui <http://blog.samsul.web.id>, dan semua tautan postingan dapat diakses seperti saat masih _dihost_ di _blogspot_.

Sekian catatan kali ini, semoga bermanfaat. Jika ada saran, pertanyaan, masukan, maupun cacian, silakan tulis di komentar blog ini. Selamat mencoba.

## Referensi
- [GitHub Pages](https://pages.github.com)
- [https://jekyllrb.com/](https://www.google.com/search?client=ubuntu&channel=fs&q=jekyll&ie=utf-8&oe=utf-8)
- [https://stackoverflow.com/questions/19878056/sed-remove-tags-from-html-file](https://stackoverflow.com/questions/19878056/sed-remove-tags-from-html-file)
- [https://kramdown.gettalong.org/syntax.html#images](https://kramdown.gettalong.org/syntax.html#images)
