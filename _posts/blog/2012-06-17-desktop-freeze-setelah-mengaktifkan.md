---
layout: post
title: Desktop Freeze Setelah Mengaktifkan WiFi
date: '2012-06-17T15:45:00.001+07:00'
author: Samsul Maarif
categories: blog
tags:
- Tutorial
modified_time: '2012-06-17T16:26:17.943+07:00'
blogger_id: tag:blogger.com,1999:blog-1383767785423682157.post-3142006451368321385
blogger_orig_url: http://blog.samsul.web.id/2012/06/desktop-freeze-setelah-mengaktifkan.html
---

Mau nulis status galau, nanti dikira ababil (beberapa orang teman menyebut demikian untuk menyingkat frase : ABG Labil). Saya kira lebih bermanfaat untuk sedikit bercerita tentang secara ringkas saja tentang judul yang sudah saya tuliskan.

Belum lama ini saya menginstall BlankOn Ombilin di laptop Acer Aspire One D257 milik seorang siswa SMA N 1 Bantarsari (Cilacap). Semua perangkat terdeteksi dengan baik oleh BlankOn Ombilin, termasuk Display, WiFi, LAN, Sound, dan lain-lain. Satu permasalahan yang timbul adalah ketika mengaktifkan WiFi desktop selalu freeze, atau istilah yang sering digunakan (utamanya di kalangan pengguna windows) adalah desktopnya "Not Responding".

Setelah gugling saya menemukan solusinya di [wiki.archlinux.org](https://wiki.archlinux.org/index.php/Acer_AO722-BZ454). Saya coba memblacklist modul bcma dan acer-wmi seperti yang disebutkan pada link tersebut :

Edit berkas /etc/modprobe.d/blacklist.conf

sudo gedit /etc/modprobe.d/blacklist.conf  


tambahkan baris berikut :

blacklist acer-wmi  
blacklist bcma  


Simpan berkas tersebut, lalu restart.  

Semudah itukah? Iya, tapi hal ini berlaku untuk keluarga Acer Aspire, beberapa yang sudah saya coba dan ternyata berhasil di seri Acer Aspire One 722, D257, dan 4739\. Kalau temen-temen mempunyai masalah yang sama dan berhasil, silahkan sampaikan melalui kolom komentar. Dalam bahasa jawanya : "Please, let me know" :D
