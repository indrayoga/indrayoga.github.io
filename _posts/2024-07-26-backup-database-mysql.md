---
layout: post
title: Membackup database mysql di server ubuntu
subtitle: tutorial membackup database mysql.
author: Indra Yoga
comments: true
tags: [mysql, database]
---

**Bismillahirrahmannirrahim.**

Ada beberapa cara untuk kita bisa membackup database mysql di server ubuntu, yang sederhana bisa dengan perintah di console/terminal:

```
mysqldump -u[USER] -p[PASSWORD] --all-databases
```

jika ingin sekaligus di compress bisa dengan perintah :

```
mysqldump -u[USER] -p[PASSWORD] --all-databases | gzip -9 > backup-all-db.sql.gz
```

jika ingin membuat penjadwalan membackup data secara periodik kita bisa menaruhnya di `cron` :

```
crontab -e
```

![konfigurasi crontab untuk backup database](/images/cron-backup-db.png)

konfigurasi diatas akan menjalankan perintah backup data setiap jam 1 malam.

untuk cara yang lebih lanjutnya kita bisa membuat skema backup yang biasa disebut _Grandfather-father-son (GFS)_ , singkatnya GFS mengharuskan kita membuat backup per bulan (_grandfather_), per minggu (_father_) dan per hari (_son_).

untuk eksekusinya kita bisa menggunakan _bash script_
[disni](https://gist.github.com/grahampugh/aa0d98968206336bfb12c78e94c88393/).

misal kita simpan _script_ tersebut di server kita dengan nama _backup-mysql.sh_, jangan lupa set mode execute pada file tersebut :

```
chmod 700 backup-mysql.sh
```

kemudian jalankan _script_ tsb:

```
./backup-mysql.sh
```

kemudian kita ubah konfigurasi _cron_ kita :

```
0 1 * * * /home/[USER]/backup-mysql.sh
```

selamat mencoba.!
