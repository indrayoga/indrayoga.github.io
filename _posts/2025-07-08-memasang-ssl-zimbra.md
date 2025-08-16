---
layout: post
title: Memasang SSL di zimbra
subtitle: langkah langkah untuk memasang SSL di server zimbra.
comments: true
tags: [server, zimbra, security]
tags: [design pattern, pemrograman]
cover-img: /assets/img/memasang-ssl-server-zimbra.png
thumbnail-img: /assets/img/memasang-ssl-server-zimbra.png
share-img: /assets/img/memasang-ssl-server-zimbra.png

image:
  path: https://indrayoga.github.io/images/memasang-ssl-server-zimbra.png
  alt: "Memasang SSL di zimbra"
---

**Bismillahirrahmannirrahim.**

# Cara Memasang SSL di Zimbra

Berikut adalah langkah-langkah memasang SSL pada server Zimbra:

1. **Akses Server Zimbra**

   Masuk ke server Zimbra menggunakan SSH.

2. **Salin File SSL**

   Salin file atau folder SSL ke direktori yang Anda miliki hak akses tulis, misalnya: `/home/indra`.

3. **Masuk sebagai User Zimbra**

   Jalankan perintah berikut untuk masuk sebagai user Zimbra:

   ```bash
   sudo su zimbra
   ```

4. **Salin File SSL ke Direktori Zimbra**

   Salin file `*.crt` dan `*.key` dari folder SSL yang sudah Anda salin pada langkah 2 ke direktori `/opt/zimbra/ssl/zimbra/commercial/`. Contoh perintah:

   ```bash
   cp /home/indra/balikpapan.crt /opt/zimbra/ssl/zimbra/commercial/
   cp /home/indra/balikpapan.key /opt/zimbra/ssl/zimbra/commercial/
   ```

5. **Masuk ke Direktori SSL Zimbra**

   Pindah ke direktori berikut:

   ```bash
   cd /opt/zimbra/ssl/zimbra/commercial/
   ```

6. **Rename File SSL**

   Ubah nama file `balikpapan.crt` menjadi `commercial.crt` dan `balikpapan.key` menjadi `commercial.key`:

   ```bash
   mv balikpapan.crt commercial.crt
   mv balikpapan.key commercial.key
   ```

   > **Catatan:** Jika sudah ada file `commercial.crt` dan `commercial.key`, sebaiknya rename atau backup terlebih dahulu.

7. **Verifikasi Kecocokan Certificate dan Key**

   Jalankan perintah berikut untuk memastikan certificate dan key cocok:

   ```bash
   /opt/zimbra/bin/zmcertmgr verifycrtkey commercial.key commercial.crt
   ```

   Jika hasilnya _match_, lanjut ke langkah berikutnya.

8. **Verifikasi Certificate Chain**

   Jalankan perintah berikut:

   ```bash
   /opt/zimbra/bin/zmcertmgr verifycrtchain commercial_ca.crt commercial.crt
   ```

   Jika hasilnya _OK_, lanjut ke langkah berikutnya.

9. **Deploy Certificate**

   Jalankan perintah berikut untuk memasang SSL:

   ```bash
   /opt/zimbra/bin/zmcertmgr deploycrt comm commercial.crt commercial_ca.crt
   ```

   Jika berhasil, proses pemasangan SSL sudah selesai.

10. **Restart Zimbra**

    Restart layanan Zimbra dengan perintah:

    ```bash
    zmcontrol restart
    ```

---

Dengan mengikuti langkah-langkah di atas, SSL pada Zimbra Anda akan terpasang dengan baik.
