---
title: Otentikasi Pada Laravel
description: Beberapa metode otentikasi yang dapat diterapkan pada laravel, baik untuk otentikasi API ataupun otentikasi yang menggunakan browser .
author: Indra Yoga Permana
date: 2024-08-09 10:36:00 +0800
pin: false
categories: [Blog]
tags: [laravel,api,PHP]
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

### auth.basic
auth.basic merupakan middleware untuk mengimplementasikan Basic access authentication yaitu teknik sederhana untuk kontrol akses suatu halaman, sangat sederhana karena kita tidak perlu membutuhkan cookes, session atau membuat halaman login sendiri.
```
Route::middleware(['auth.basic'])->group(function () {
   Route::get('home', function () {
       echo "<h1>Selamat Datang</h1>";
   });
});
```
ketika mengakses halaman yang menggunakan middleware ini melalui browser, browser akan menampilkan form login

![halaman form login ketika menggunakan auth.basic](/images/image.png)

di laravel ketika menggunakan metode ini, form username akan menganggap ke kolom email di tabel users. 

selain di browser, metode basic authentication ini juga bisa untuk digunakan otentikasi API.

```
<?php

$username = "indra@email.com";
$password = "secret";

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'localhost:8000/dashboard',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Authorization: basic' . base64_encode([$username] . ':' . $password)
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```

### [auth.sanctum](https://laravel.com/docs/11.x/sanctum#main-content)
Yang terakhir adalah [auth.sanctum](https://laravel.com/docs/11.x/sanctum#main-content), middleware yang mengimplementasikan otentikasi berbasis token, berbeda dengan auth dan auth.basic kita tidak bisa langsung memakainya tapi kita harus menginstall package nya terlebih dahulu.
```
php artisan install:api
```

kemudian kita bisa menggunakannya sebagaimana auth dan auth.basic :
```
Route::middleware(['auth.sanctum'])->group(function () {
   Route::get('home', function () {
       echo "<h1>Selamat Datang</h1>";
   });
});
```

tidak seperti auth yang menggunakan cookies atau session, auth.sanctum menggunakan api token yang di pasang di header request seperti halnya auth.basic tapi lebih aman karena tidak menyertakan username atau password pada setiap request melainkan menggunakan token.

```
<?php

$username = "indra@email.com";
$password = "secret";

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'localhost:8000/dashboard',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Authorization: basic' . base64_encode([$username] . ':' . $password)
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```
