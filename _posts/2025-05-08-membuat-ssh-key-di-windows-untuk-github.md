---
title: Menggunakan SSH Key di Windows Untuk github
description: Cara membuat SSH Key di windows dan memasangnya di github SSH Key
author: Indra Yoga Permana
date: 2025-05-08 10:00:00 +0800
pin: true
comments: true
categories: [Blog]
tags: [github,windows]
image:
  path: https://indrayoga.github.io/images/ssh-keys-github.png
  alt: memasang SSH Key di github
---

**Bismillahirrahmannirrahim.**

Biasanya, saya menggunakan SSH di Linux atau melalui WSL (Windows Subsystem for Linux) jika menggunakan sistem operasi Windows. Namun, karena sekarang Windows sudah menyertakan OpenSSH secara bawaan, saya mencoba membuat SSH key langsung di Windows dan menggunakannya untuk keperluan GitHub.

Berikut adalah langkah-langkah yang saya lakukan:
## 1. Membuat SSH Key

Buka **Command Prompt** atau **PowerShell**, lalu jalankan perintah berikut:

```bash
ssh-keygen -t ed25519 -C "email@contoh.com"
```

Gantilah `email@contoh.com` dengan email GitHub Anda. Ikuti petunjuk selanjutnya di layar. Biasanya, Anda hanya perlu menekan `Enter` untuk menerima lokasi default penyimpanan dan memilih apakah ingin menambahkan passphrase atau tidak.

## 2. Mengaktifkan SSH Agent di Windows

Secara default, layanan **SSH Agent** di Windows tidak aktif. Untuk mengaktifkannya:

1. Buka **Start Menu** lalu ketik `services`, kemudian buka aplikasi **Services**, Atau, Anda bisa membukanya melalui **command prompt** dengan menjalankan perintah 
    ```bash
      services.msc
    ```

2. Cari *`service`* bernama **OpenSSH Authentication Agent**.

3. Klik dua kali *`service`* tersebut, lalu pada bagian **Startup Type**, pilih **Automatic** atau **Manual**.

4. Klik **Apply**, kemudian klik **Start** untuk menjalankan *servicenya*.

## 3. Menambahkan SSH Key ke SSH Agent

Kembali ke **Command Prompt** atau **PowerShell**, jalankan perintah berikut untuk menambahkan `private key` Anda ke **SSH agent**:

```bash
ssh-add ~/.ssh/id_ed25519
```

Pastikan Anda menggunakan path yang sesuai jika Anda menyimpan `SSH key` di lokasi yang berbeda.

## 4. Menambahkan Public Key ke GitHub

1. Buka file `~/.ssh/id_ed25519.pub` menggunakan text editor (misalnya Notepad).

2. Salin seluruh isi file tersebut.

3. Masuk ke akun GitHub Anda.

3. Pergi ke `Settings > SSH and GPG keys`.

4. Klik `New SSH key`, beri nama (misalnya: Windows Laptop), lalu tempelkan `key` yang tadi disalin.

5. Klik `Add SSH key`.

Dengan langkah-langkah di atas, Anda sudah dapat menggunakan koneksi SSH saat berinteraksi dengan GitHub dari Windows.

