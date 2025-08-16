---
layout: post
title: Menggunakan SSH Key di Windows Untuk github
subtitle: Cara membuat SSH Key di windows dan memasangnya di github SSH Key
comments: true
tags: [github, windows]
cover-img: /assets/img/ssh-keys-github.png
thumbnail-img: /assets/img/ssh-keys-github.png
share-img: /assets/img/ssh-keys-github.png
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

2. Cari _`service`_ bernama **OpenSSH Authentication Agent**.

3. Klik dua kali _`service`_ tersebut, lalu pada bagian **Startup Type**, pilih **Automatic** atau **Manual**.

4. Klik **Apply**, kemudian klik **Start** untuk menjalankan _servicenya_.

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

4. Pergi ke `Settings > SSH and GPG keys`.

5. Klik `New SSH key`, beri nama (misalnya: Windows Laptop), lalu tempelkan `key` yang tadi disalin.

6. Klik `Add SSH key`.

Dengan langkah-langkah di atas, Anda sudah dapat menggunakan koneksi SSH saat berinteraksi dengan GitHub dari Windows.
