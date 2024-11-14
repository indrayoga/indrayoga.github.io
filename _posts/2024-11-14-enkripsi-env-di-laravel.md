---
title: Enkripsi Berkas .env di Laravel
description: Mengenkripsi berkas .env agar bisa di simpan di source control dengan aman
author: Indra Yoga Permana
date: 2024-11-14 10:23:00 +0800
pin: false
comments: true
categories: [Blog]
tags: [laravel,PHP]
---

**Bismillahirrahmannirrahim.**

# Fungsi berkas .env di Laravel

Berkas `.env` di Laravel berfungsi untuk menyimpan konfigurasi aplikasi yang bersifat sensitif dan dapat berbeda pada setiap lingkungan (*environment*). Misalnya, konfigurasi di lingkungan *development* biasanya berbeda dengan di lingkungan *production*. Umumnya, berkas `.env` tidak disertakan dalam *source control* karena mengandung informasi sensitif dan rahasia.

Namun, jika dalam kondisi tertentu kita perlu menyertakan berkas `.env` di *source control*, Laravel menyediakan fitur enkripsi untuk berkas ini. Dengan begitu, kita dapat menyimpan berkas `.env` secara aman. 

# Cara Mengenkripsi berkas .env
Untuk mengenkripsi berkas `.env`, kita dapat menjalankan perintah berikut:

```bash
php artisan env:encrypt
```

Setelah menjalankan perintah ini, Anda akan diminta memilih jenis kunci enkripsi yang akan digunakan. Laravel menyediakan opsi untuk membuat kunci secara manual atau di-generate secara acak oleh sistem.

![pilih kunci enkripsi](/images/pilih-kunci-enkripsi.png)

Jika proses enkripsi berhasil, berkas baru bernama `.env`.encrypted akan terbentuk. berkas inilah yang dapat kita sertakan di source control dengan aman.

# Cara Mendekripsi berkas .env

Untuk mengembalikan berkas `.env` ke format asli (plain text), jalankan perintah berikut:
```bash
php artisan env:decrypt
```
dan masukkan kunci enkripsi yang sudah dibuat saat mengenkripsi berkas `.env` sebelumnya.

Perintah ini akan mengekstrak kembali isi berkas `.env` yang terenkripsi dan menampilkannya dalam format asli.

Dengan cara ini, informasi sensitif dalam berkas `.env` dapat tetap aman meskipun harus disertakan di *source control*.
