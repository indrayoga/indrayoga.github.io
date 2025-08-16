---
layout: post
title: Belajar Design Pattern Singleton
subtitle: Singleton adalah pola desain yang memastikan bahwa sebuah kelas hanya memiliki satu _instance_ saja selama siklus hidup aplikasi, dan menyediakan cara global untuk mengakses _instance_ tersebut
comments: true
tags: [design pattern, pemrograman]
cover-img: /assets/img/design-pattern-singleton.png
thumbnail-img: /assets/img/design-pattern-singleton.png
share-img: /assets/img/design-pattern-singleton.png
---

**Bismillahirrahmannirrahim.**

# Design Pattern Pertama: Singleton

Design pattern pertama yang saya pelajari adalah **Singleton**. Menurut saya, pola desain ini adalah salah satu yang paling sering digunakan dalam pengembangan perangkat lunak. Mengapa demikian? Karena salah satu implementasi paling umum dari Singleton adalah pada saat kita menulis kode untuk koneksi ke database.

---

## Apa itu Singleton?

> **Singleton** adalah pola desain yang memastikan bahwa sebuah kelas hanya memiliki satu _instance_ saja selama siklus hidup aplikasi, dan menyediakan cara global untuk mengakses _instance_ tersebut.  
> **Sumber referensi:** [Refactoring.Guru - Singleton Pattern](https://refactoring.guru/design-patterns/singleton)

---

## Kenapa Harus Singleton?

Untuk memudahkan pemahaman, mari kita analogikan dengan **printer di kantor**. Tidak mungkin setiap karyawan diberi satu printer pribadi — cukup satu printer saja yang bisa digunakan bersama. Semua orang tetap bisa mencetak dokumen, tapi yang digunakan hanya satu printer secara bersama-sama.

Begitu juga dalam pemrograman. Misalnya saat membuat koneksi ke database, tanpa pola Singleton, kita bisa saja membuat koneksi baru setiap kali dibutuhkan. Padahal, dalam banyak kasus, satu koneksi yang dikelola dengan baik sudah cukup dan lebih efisien.

Dengan menerapkan **Singleton**, kita bisa memastikan hanya ada satu koneksi yang digunakan bersama oleh seluruh bagian aplikasi, sehingga lebih hemat sumber daya dan lebih stabil.

## Mari kita lihat contoh kode berikut:

```php
<?php

class KoneksiDatabase
{
    private $servername = "localhost";
    private $username = "username";
    private $password = "password";
    private $dbname = "database";
    private $conn;

    public function __construct()
    {
        $this->conn = new mysqli($this->servername, $this->username, $this->password, $this->dbname);
    }

    public function getConnection()
    {
        return $this->conn;
    }
}

```

Sekilas, kode di atas tampak baik-baik saja. Namun perhatikan skenario berikut:

> file a.php

```php
<?php
  $koneksi = new KoneksiDatabase();
```

> file b.php

```php
<?php
  $koneksi = new KoneksiDatabase();
```

Jika aplikasi dijalankan dan kedua file (`a.php` dan `b.php`) dipanggil dalam satu proses, maka **akan dibuat dua objek `KoneksiDatabase` terpisah**. Artinya, **dua koneksi database aktif**, yang mungkin **tidak diperlukan** dan justru membuang-buang resource.

Padahal kita bisa membuat **satu objek saja**, dan **menggunakannya kembali** di berbagai bagian aplikasi. Inilah tujuan utama dari penerapan pola **Singleton**.

## Ciri-Ciri Umum Pola Singleton

Agar sebuah kelas memenuhi pola desain **Singleton**, biasanya memiliki ciri-ciri berikut:

- **Constructor bersifat** `private` untuk mencegah kode di luar kelas untuk membuat objek baru secara langsung.

- **Memiliki method** `getInstance()` untuk mengakses satu-satunya instance dari kelas tersebut.

- **Instance disimpan dalam variabel** `static`
  Variabel ini berada di dalam kelas itu sendiri, dan digunakan untuk menyimpan serta mengontrol akses ke instance yang tunggal.

dari ciri ciri umum pola di atas kita bisa sesuaikan kelas `KoneksiDatabase` sebelumnya agar sesuai dengan pola Singleton:

```php
class KoneksiDatabase
{
    private $servername = "localhost";
    private $username = "username";
    private $password = "password";
    private $dbname = "database";
    private $conn;
    private static ?self $instance = null;

    private function __construct()
    {
        $this->conn = new mysqli($this->servername, $this->username, $this->password, $this->dbname);
    }

    public static function getInstance()
    {
        if (self::$instance == null) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    public function getConnection()
    {
        return $this->conn;
    }
}

```

Setelah kelas diubah sesuai pola Singleton, kita cukup memanggil instance-nya seperti ini:

```php
$koneksi = KoneksiDatabase::getInstance();
```

Dan untuk mendapatkan koneksinya:

```php
$conn = $koneksi->getConnection();
```

Dengan pendekatan ini, hanya akan ada satu objek koneksi database selama aplikasi berjalan. Efisien dan sesuai dengan prinsip resource management yang baik.
