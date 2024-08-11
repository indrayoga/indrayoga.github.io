---
title: Kustom konfigurasi header untuk otorisasi API di SPLP
description: Konfigurasi header untuk otoriasi API berbasis token atau apikey bagi pengguna laravel sanctum atau aplikasi yang menggunakan otentikasi berbasis token.
author: Indra Yoga Permana
date: 2024-08-10 22:23:00 +0800
pin: true
comments: true
categories: [Blog]
tags: [laravel, PHP, API, SPLP]
---

**Bismillahirrahmannirrahim.**

**SPLP** atau Sistem Penghubung Layanan Pemerintah merupakan sistem untuk mendukung interoperabilitas data antar layanan Pemerintah bersekala Nasional.

jadi, ada kasus dimana saya harus memasukkan *API* aplikasi yang menggunakan *API token* untuk sistem otentikasinya ke dalam SPLP, aplikasi tersebut menggunakan *framework laravel* dan *package laravel sanctum* untuk sistem otentikasinya, jadi biasanya laravel *sanctum* menggenerate *token* yang tidak pernah *expire* kecuali di *invalidated*, *token* tersebut di letakkan di *Authorization header* sebagai *bearer token*.

Setelah saya coba untuk memasukkan *endpoint* aplikasi tersebut, tidak ada pilihan untuk memasukkan *token* tersebut

![pilihan otentikasi endpoint di splp](/images/pilihan-otentikasi-endpoint-splp.png)

hanya ada autentikasi dasar, digest otentikasi dan *oauth 2.0*, ketiganya bukanlah pilihan yang cocok untuk otentikasi berbasis *token* seperti yang digunakan aplikasi tersebut.

setelah melakukan pencarian dan bertanya saya dapat solusinya, yaitu dengan membuat *custom header*, caranya sebagai berikut :

1. masuk ke API Configurations, kemudian pilih Runtime
2. aktifkan Konfigurasi CORS
   ![aktifkan konfigurasi CORS SPLP](/images/konfigurasi-cors-splp.png)

3. setelah itu pilih Mediasi Pesan
   ![mediasi pesan](/images/mediasi-kebijakan-splp.png)
   
4. pilih kebijakan kustom
   ![kebijakan kustom](/images/kebijakan-kustom-splp.png)
   
5. buat file xml dengan isi sebagai berikut :

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <sequence name="add_custom_header" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
   <header name="Authorization" value="Bearer [ISI TOKEN APLIKASI]" scope="transport"/>
   </sequence>
   ```

   save dengan nama add_custom_header.xml

7. unggah file tersebut pada form kebijakan kustom sebagaimana pada langkah ke 4.
8. setelah itu pilih save and deploy.

dan selesai, akhirnya saya bisa memasukkan endpoint aplikasi tersebut kedalam SPLP.
