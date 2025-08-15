---
layout: post
title: Kustom konfigurasi header untuk otorisasi API di SPLP
subtitle: Konfigurasi header untuk otoriasi API berbasis token atau apikey bagi pengguna laravel sanctum atau aplikasi yang menggunakan otentikasi berbasis token.
author: Indra Yoga
comments: true
tags: [laravel, PHP, API, SPLP]
---

**Bismillahirrahmannirrahim.**

**SPLP** atau Sistem Penghubung Layanan Pemerintah merupakan sistem untuk mendukung interoperabilitas data antar layanan Pemerintah bersekala Nasional.

jadi, ada kasus dimana saya harus memasukkan _API_ aplikasi yang menggunakan _API token_ untuk sistem otentikasinya ke dalam SPLP, aplikasi tersebut menggunakan _framework laravel_ dan _package laravel sanctum_ untuk sistem otentikasinya, jadi biasanya laravel _sanctum_ menggenerate _token_ yang tidak pernah _expire_ kecuali di _invalidated_, _token_ tersebut di letakkan di _Authorization header_ sebagai _bearer token_.

Setelah saya coba untuk memasukkan _endpoint_ aplikasi tersebut, tidak ada pilihan untuk memasukkan _token_ tersebut

![pilihan otentikasi endpoint di splp](/images/pilihan-otentikasi-endpoint-splp.png)

hanya ada autentikasi dasar, digest otentikasi dan _oauth 2.0_, ketiganya bukanlah pilihan yang cocok untuk otentikasi berbasis _token_ seperti yang digunakan aplikasi tersebut.

setelah melakukan pencarian dan bertanya saya dapat solusinya, yaitu dengan membuat _custom header_, caranya sebagai berikut :

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

6. unggah file tersebut pada form kebijakan kustom sebagaimana pada langkah ke 4.
7. setelah itu pilih save and deploy.

dan selesai, akhirnya saya bisa memasukkan endpoint aplikasi tersebut kedalam SPLP.
