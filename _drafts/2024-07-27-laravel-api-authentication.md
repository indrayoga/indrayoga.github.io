---
title: API Authentication Pada Laravel
description: Beberapa metode *API Authentication* yang dapat diterapkan pada laravel.
author: Indra Yoga Permana
date: 2024-07-27 22:27:00 +0800
pin: false
categories: [Blog]
tags: [laravel]
---

Laravel memiliki beberapa middleware otentikasi bawaan yang bisa digunakan, yaitu :

### auth
Middleware ini adalah mungkin yang paling sering digunakan, middleware ini digunakan ketika akan menggunakan otentikasi berbasis cookie
```
Route::middleware(['auth'])->group(function () {
   Route::get('home', function () {
       echo "<h1>Selamat Datang</h1>";
   });
});
```
untuk menggunakan middleware ini sebelumnya kita harus mengimplementasikan sistem otentikasi, membuat halaman login dll, atau kita bisa menggunakan package yang sudah disediakan oleh laravel seperti laravel breeze dan laravel jetstream.
