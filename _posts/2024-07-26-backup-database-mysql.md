---
title: Membackup database mysql di server ubuntu
description: tutorial membackup database mysql.
author: Indra Yoga Permana
date: 2024-07-26 14:35:00 +0800
pin: true
categories: [Blog]
tags: [server,database,mysql]
---

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

konfigurasi diatas akan menjalankan perintah backup data setiap jam 9 malam.

untuk cara yang lebih lanjutnya kita bisa membuat skema backup yang biasa disebut *Grandfather-father-son (GFS)* , singkatnya GFS mengharuskan kita membuat backup per bulan (*grandfather*), per minggu (*father*) dan per hari (*son*).

untuk eksekusinya kita bisa menggunakan *bash script* 
[disni](https://gist.github.com/grahampugh/aa0d98968206336bfb12c78e94c88393/).

misal kita simpan *script* tersebut di server kita dengan nama *backup-mysql.sh*, jangan lupa set mode execute pada file tersebut :
```
chmod 700 backup-mysql.sh
```
kemudian jalankan *script* tsb:
```
./backup-mysql.sh
```
kemudian kita ubah konfigurasi *cron* kita :
```
0 1 * * * /home/[USER]/backup-mysql.sh
```

selamat mencoba.!



