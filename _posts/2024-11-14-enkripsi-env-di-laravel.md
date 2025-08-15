---
layout: post
title: Enkripsi Berkas .env di Laravel
subtitle: Mengenkripsi berkas .env agar bisa di simpan di source control dengan aman
author: Indra Yoga
comments: true
tags: [laravel, PHP]
---

**Bismillahirrahmannirrahim.**

# Fungsi berkas .env di Laravel

Berkas `.env` di Laravel berfungsi untuk menyimpan konfigurasi aplikasi yang bersifat sensitif dan dapat berbeda pada setiap lingkungan (_environment_). Misalnya, konfigurasi di lingkungan _development_ biasanya berbeda dengan di lingkungan _production_. Umumnya, berkas `.env` tidak disertakan dalam _source control_ karena mengandung informasi sensitif dan rahasia.

Namun, jika dalam kondisi tertentu kita perlu menyertakan berkas `.env` di _source control_, Laravel menyediakan fitur enkripsi untuk berkas ini. Dengan begitu, kita dapat menyimpan berkas `.env` secara aman.

# Cara Mengenkripsi berkas .env

Untuk mengenkripsi berkas `.env`, kita dapat menjalankan perintah berikut:

```bash
php artisan env:encrypt
```

Setelah menjalankan perintah ini, Anda akan diminta memilih jenis kunci enkripsi yang akan digunakan. Laravel menyediakan opsi untuk membuat kunci secara manual atau di-generate secara acak oleh sistem.

![pilih kunci enkripsi](/images/pilih-kunci-enkripsi.png)

Jika proses enkripsi berhasil, berkas baru bernama `.env.encrypted` akan terbentuk. berkas inilah yang dapat kita sertakan di _source control_ dengan aman.

# Cara Mendekripsi berkas .env

Untuk mengembalikan berkas `.env` ke format asli (_plain text_), jalankan perintah berikut:

```bash
php artisan env:decrypt
```

dan masukkan kunci enkripsi yang sudah dibuat saat mengenkripsi berkas `.env` sebelumnya.

Perintah ini akan mengekstrak kembali isi berkas `.env` yang terenkripsi dan menampilkannya dalam format asli.

Dengan cara ini, informasi sensitif dalam berkas `.env` dapat tetap aman meskipun harus disertakan di _source control_.
