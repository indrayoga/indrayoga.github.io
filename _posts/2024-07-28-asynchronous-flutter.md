---
layout: post
title: Asynchronous di Flutter
subtitle: Penggunaan fungsi Asynchronous di flutter.
author: Indra Yoga
comments: true
tags: [flutter]
---

**Bismillahirrahmannirrahim.**

Semisal kita punya baris kode seperti berikut :

```
void main() {
  print("Nama : Indra");
  print("Mulai Berhitung");
  for (int i = 0; i < 10; i++) {
    print('hello ${i + 1}');
  }
  print("Selesai Berhitung");
}
```

maka _flutter_ akan mengeksekusi baris kode dan menampilkan hasil tersebut mulai dari atas sampai bawah secara berurutan.

```
Nama : Indra
Mulai Berhitung
hello 1
hello 2
hello 3
hello 4
hello 5
hello 6
hello 7
hello 8
hello 9
hello 10
Selesai Berhitung
```

dan ini yang disebut _synchronous_, untuk membuat menjadi _asynchronous_ kita bisa bungkus kode tersebut dengan fungsi _Future()_, perhatikan kode berikut :

```
void main() {
  print("Nama : Indra");
  Future((){
    print("Mulai Berhitung");
  });
  for (int i = 0; i < 10; i++) {
    print('hello ${i + 1}');
  }
  print("Selesai Berhitung");
}
```

coba kita jalankan dan lihat hasilnya :

```
Nama : Indra
hello 1
hello 2
hello 3
hello 4
hello 5
hello 6
hello 7
hello 8
hello 9
hello 10
Selesai Berhitung
Mulai Berhitung
```

bisa kita lihat kode yang di bungkus oleh _Future()_ tampil terakhir padahal di urutan baris kodenya seperti itu, ini dikarenakan baris ` Future((){
    print("Mulai Berhitung");
  });` dijalankan di _thread_ berbeda dengan _main thread_.

# untuk apa asynchronous

Pada contoh kode diatas mungkin tidak terlalu terlihat kegunaan dari asynchronous. Akan tetapi untuk melakukan pekerjaan yang berat, atau pekerjaan yang membutuhkan waktu yang belum dapat kita pastikan kapan pekerjaan itu selesai maka fungsi asynchronous ini akan sangat bermanfaat, salah satu contohnya adalah ketika melakukan permintaan data (request) misal melakukan http request pada API, contoh pada kode berikut :

```
void main() {
  fetchDatapegawai();
  print("Mengambil data pegawai");
}

Future<void> fetchDatapegawai(){
  //anggap kita sedang melakukan http request API data pegawai
  //karena data yang banyak dan juga melalui jaringan maka pemroses cukup memakan waktu
  return Future.delayed(Duration(seconds: 5),()=>print("tampilkan data user"));
}
```

# Async dan Await

Penggunaan fungsi asynchronous biasanya disertai dengan 2 _keyword_ yaitu `async` dan `await`, ada 2 aturan dalam penggunaan _keyword_ ini :

1. `async` diletakkan setelah nama fungsi atau sebelum tanda kurung kurawal pembuka `{`.
2. `await` hanya bisa dipakai di fungsi yang menggunakan _keyword_ `async`.

```
Future getAllUser() async {
    var response = await http.get(Uri.parse("https://reqres.in/api/users"));
  }
```

untuk contoh lebih lengkap dan bisa dijalankan bisa check di [github](https://github.com/indrayoga/flutter_async_example) saya.
