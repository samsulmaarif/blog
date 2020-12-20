---
layout: post
title: Migrasikan Konten Django Lama ke Jekyll
date: '2020-12-20'
author: Samsul Maarif
categories: blog
tags:
- BlankOn
- Linux
cover-img:
  - "/img/ipin-junior-sysadmin-2018-11-27.jpg" : "Ipin Junior SysAdmin (2018)"
share-img: "/img/ipin-junior-sysadmin-2018-11-27.jpg"
---

Para pengembang BlankOn Linux saat ini telah menggunakan Jekyll untuk
beberapa situsnya, salah satunya adalah laman <http://blankon.id>.
Penulisan konten berbasis Markdown dan situs statis merupakan pilihan
yang diambil setelah beberapa tahun sebelumnya para pendahulu
menggunakan Django untuk sebagian besar website mereka. Konsekuensinya
konten-konten website lama yang berbasis Django tersebut perlu
dimigrasikan ke format yang sesuai.

Pada web lama yang berbasis Django tersebut, diketahui bahwa basisdata
yang digunakan adalah sqlite3.

![DBeaver sqlite3](https://i.imgur.com/pWTz4WP.png)

Catatan ini menceritakan bagaimana saya mengonversi data yang berada di
basisdata sqlite tersebut ke dalam format berkas markdown. Langkah
pertama yang saya lakukan adalah menulis skrip berbasis python dan
memasang beberapa peralatan yang diperlukan:

```
# Buat virtual environment bernama 'venv'
python3 -m venv venv
# Aktifkan
source venv/bin/activate
# pastikan versi python yang digunakan adalah 3.x
python -V
# pasang modul html2text untuk mengonversi html ke markdown
pip install html2text
# buat sebuah script bernama convert.py
vim convert.py
```

Konten dari berkas `convert.py` adalah sebagai berikut:

```
#!/usr/bin/env python3
#
import sqlite3
# from sqlite3 import Error
import os
import html2text
import re

con = sqlite3.connect('blankonweb.db')

# def sql_connection():
#     try:
#         con = sqlite3.connect('mydatabase.db')
#         return con
#     except Error:
#         print(Error)

def sql_fetch(con):
    cursorObj = con.cursor()
    cursorObj.execute('SELECT id, post_date, post_title, post_entry FROM berita_post')
    rows = cursorObj.fetchall()

    for row in rows:
        # id = row[0]
        date, time = row[1].split(' ')
        year, month, day = date.split('-')
        # tanggal
        title = row[2]
        content = row[3]
        md_content = html2text.html2text(content)
        title_clean = re.sub(r'[^\w\s]','', title)
        slug = title_clean.replace(' ', '-').lower()
        filename = '%s-%s-%s-%s' % (year,month,day,slug)
        os.makedirs('_posts', exist_ok=True)
        new_file = open('_posts/%s.md' % filename, 'w')
        header = '''---
author: Humas
editor: Humas
date: "%s"
layout: post
license: CC-BY-SA-3.0
title: "%s"
image: /assets/images/omw.png
categories:
- berita
tags:
- berita
---

%s
    ''' % (date, title_clean, md_content)
        new_file.write(header)
        new_file.close()

sql_fetch(con)

```

Jalankan script tersebut:

```
python convert.py
```

Oiya, pastikan berkas sqlite `blankonweb.db` berada di direktori yang
sama dengan skrip tersebut. Hasilnya akan berada di direktori `_posts`
berupa format markdown yang dapat kita gunakan untuk ditempel ke
direktori jekyll sebagai konten postingan.

Hasil dari skrip ini lumayan membuat saya puas. Setelah sekian lama
belajar `python` secara kurang serius, akhirnya saya dapat menggunakan
sedikit pemahaman bahasa ini untuk mengerjakan tugas konversi konten
ini. Beberapa tautan mungkin masih perlu disesuaikan, terutama tautan
yang sudah tidak dapat diakses lagi.

Catatan ini sebelumnya diterbitkan [di sini](https://wiki.samsul.web.id/linux/BlankOn/Migrasikan.Konten.Django.Lama.ke.Jekyll).

## Referensi
-   <https://griff.steni.us/blog/2017/01/13/from_zinnia_to_jekyll.html>
-   <https://likegeeks.com/python-sqlite3-tutorial/>
-   <https://www.tutorialspoint.com/python/python_strings.htm>
