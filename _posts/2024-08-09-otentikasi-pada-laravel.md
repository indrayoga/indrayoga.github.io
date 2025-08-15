---
layout: post
title: Otentikasi Pada Laravel
subtitle: Beberapa metode otentikasi yang dapat diterapkan pada laravel, baik untuk otentikasi API ataupun otentikasi yang menggunakan browser .
author: Indra Yoga
tags: [laravel, api, PHP]
---

**Bismillahirrahmannirrahim.**
Laravel memiliki beberapa _middleware_ otentikasi bawaan yang bisa digunakan, yaitu :

### _auth_

_Middleware_ ini adalah mungkin yang paling sering digunakan, _middleware_ ini digunakan ketika akan menggunakan otentikasi berbasis _cookie_

```
Route::middleware(['auth'])->group(function () {
   Route::get('home', function () {
       echo "<h1>Selamat Datang</h1>";
   });
});
```

untuk menggunakan _middleware_ ini sebelumnya kita harus mengimplementasikan sistem otentikasi, membuat halaman login dll, atau kita bisa menggunakan package yang sudah disediakan oleh laravel seperti laravel _breeze_ dan laravel _jetstream_.

### _auth.basic_

_auth.basic_ merupakan middleware untuk mengimplementasikan Basic access authentication yaitu teknik sederhana untuk kontrol akses suatu halaman, sangat sederhana karena kita tidak perlu membutuhkan cookes, session atau membuat halaman login sendiri.

```
Route::middleware(['auth.basic'])->group(function () {
   Route::get('home', function () {
       echo "<h1>Selamat Datang</h1>";
   });
});
```

ketika mengakses halaman yang menggunakan middleware ini melalui _browser_, _browser_ akan menampilkan _form login_

![halaman form login ketika menggunakan auth.basic](/images/form-login.png)

di laravel ketika menggunakan metode ini, form _username_ akan menganggap ke kolom _email_ di tabel _users_.

selain di _browser_, metode _basic authentication_ ini juga bisa untuk digunakan otentikasi API.

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

### [_auth:sanctum_](https://laravel.com/docs/11.x/sanctum#main-content)

Yang terakhir adalah [_auth:sanctum_](https://laravel.com/docs/11.x/sanctum#main-content), _middleware_ yang mengimplementasikan otentikasi berbasis _token_, berbeda dengan _auth_ dan _auth.basic_ kita tidak bisa langsung memakainya tapi kita harus menginstall package nya terlebih dahulu.

```
php artisan install:api
```

kemudian kita bisa menggunakannya sebagaimana _auth_ dan _auth.basic_ :

```
Route::middleware(['auth:sanctum'])->group(function () {
   Route::get('home', function () {
       echo "<h1>Selamat Datang</h1>";
   });
});
```

tidak seperti _auth_ yang menggunakan _cookies_ atau _session_, _auth.sanctum_ menggunakan _API token_ yang di pasang di _header request_ seperti halnya _auth.basic_ tapi lebih aman karena tidak menyertakan _username_ atau _password_ pada setiap request melainkan menggunakan _token_.

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
    'Authorization: bearer [token]'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```
