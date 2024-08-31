---
title: "Perintah Command Line Untuk SysAdmin Linux #2 - monitoring log dengan `tail`"
description: Berikut ini beberapa perintah command line di server linux yang saya pelajari dan biasa saya gunakan selama menjadi system administrator 
author: Indra Yoga Permana
date: 2024-08-20 21:00:00 +0800
pin: false
comments: true
categories: [Blog]
tags: [linux,ubuntu]
---

**Bismillahirrahmannirrahim.**

### Monitoring Log `tail`

Baiklah, sebenarnya fungsi `tail` adalah untuk menampilkan sejumlah baris terakhir dari sebuah file, secara `default` jumlah baris terakhir yang ditampilkan adalah 10 baris terakhir, jika punya file dengan alamat file di `/var/log/apache2/access.log`, jika kita jalankan perintah
```
tail /var/log/apache2/access.log
```
dilayar akan tampil 10 baris terakhir dari file access.log tersebut.
![hasil keluaran dari perintah tail](/images/hasil-perintah-tail.png)

jika ingin menampilkan lebih atau kurang dari 10 kita bisa tambahkan *option* `-n`
```
tail -n 20 /var/log/apache2/access.log
```
kita bisa tambahkan *option* `-n`
```
tail -n 5 /var/log/apache2/access.log
```

dan ada satu *option* yaitu *option* `-f` yang biasa di gunakan system administrator untuk memonitor penambahan log secara *realtime*
```
tail -f /var/log/apache2/access.log
```
dan kita bisa melihat pergerakan penambahan log secara realtime di layar komputer apabila ada yang mencoba mengakses halaman website kita.
