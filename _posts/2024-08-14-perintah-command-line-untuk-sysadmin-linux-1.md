---
layout: post
title: "Perintah Command Line Untuk SysAdmin Linux #1 - mengetahui ukuran file/direktori dengan du"
subtitle: Berikut ini beberapa perintah command line di server linux yang saya pelajari dan biasa saya gunakan selama menjadi system administrator
author: Indra Yoga
comments: true
tags: [linux, ubuntu]
---

**Bismillahirrahmannirrahim.**

### Mengetahui ukuran suatu direktori dengan perintah `du`

Semakin lama server kita akan semakin banyak file file baru yang bertambah dan kapasitas penyimpanan harddisk akan berkurang, pastinya kita ingin tahu dimana atau apa yang banyak menggunakan kapasitas penyimpanan harddisk server kita, perintah `du` bisa digunakan untuk menghitung ukuran file atau direktori.

Contoh :

```
du ~
```

akan menampilkan ukuran file atau direktori di direktori /home kita secara recursive sampai ke file atau direktori terakhir, jika ingin summary dari satu direktori /home, tambahkan argument `-s`

```
du -s ~
```

jika ingin ukurannya lebih friendly, tambahkan argument h pada perintah du tadi.

```
du -sh ~
```

perintah tersebut akan menampilkan ukuran file/direktori yang lebih mudah dimengerti, jika ingin menampilkan secara recursive, kita bisa membatasi sedalam apa direktori akan di akses dengan menambahkan argumen `--max-depth`

```
du -h --max-depth=1 ~
```
