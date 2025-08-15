---
layout: post
title: "GetStorage Flutter: Cara Menyimpan dan Membaca Custom Objek"
subtitle: "Menyimpan custom object di getstorage flutter dan membuat helper yang bisa membantu mengurangi boilerplate."
comments: true
tags: [flutter]
image:
  path: https://indrayoga.github.io/images/Menyimpan-Objek-Kustom-di-Flutter.png
  alt: Menyimpan-Objek-Kustom-di-Flutter
---

**Bismillahirrahmannirrahim.**

## Membuat Helper Untuk Memudahkan Proses Write dan Read Data ke GetStorage

Agar proses menyimpan (_write_) dan mengambil (_read_) data ke **_GetStorage_**, saya membuat sebuah **Helper Class** sederhana untuk memudahkan dalam penulisan kode aplikasi,_Helper_ yang saya buat seperti berikut :

```dart
import 'package:get_storage/get_storage.dart';

class StorageService {
  final GetStorage _box = GetStorage();

  /// Saves a key-value pair to the storage.
  ///
  /// This method writes the provided [key] and [value] to the storage asynchronously.
  ///
  /// - [key]: A `String` representing the key under which the value will be stored.
  /// - [value]: A dynamic value to be stored. It can be of any type.
  ///
  /// Throws an exception if the write operation fails.
  Future<void> saveData(String key, dynamic value) async {
    await _box.write(key, value);
  }

  /// Reads and retrieves data of type [T] associated with the given [key] from storage.
  ///
  /// This method uses a generic type [T] to specify the expected type of the
  /// returned data. If no data is found for the provided [key], it returns `null`.
  ///
  /// - Parameter [key]: The key used to identify the stored data.
  /// - Returns: The data of type [T] if it exists, or `null` if no data is found.
  T? readData<T>(String key) {
    return _box.read<T>(key);
  }

  /// Removes the data associated with the given [key] from the storage.
  ///
  /// This method asynchronously deletes the entry identified by [key]
  /// from the underlying storage box.
  ///
  /// [key]: The key of the data to be removed.
  Future<void> removeData(String key) async {
    await _box.remove(key);
  }

  /// Checks if the storage contains data associated with the given [key].
  ///
  /// Returns `true` if the data exists, otherwise `false`.
  ///
  /// [key]: The key to check for existence in the storage.
  bool hasData(String key) {
    return _box.hasData(key);
  }

  /// Clears all data from the storage.
  ///
  /// This method asynchronously erases all entries in the underlying
  /// storage box, effectively resetting it to an empty state.
  Future<void> clearStorage() async {
    await _box.erase();
  }
}

```

Dengan helper ini, saya bisa menyimpan dan mengambil data dengan tipe apa pun, termasuk objek. Misalnya, saya memiliki model `User` seperti berikut:

```dart
import 'dart:convert';

User userFromJson(String str) => User.fromJson(json.decode(str));

String userToJson(User data) => json.encode(data.toJson());

class User {
  String name;
  String email;
  String accessToken;
  String tokenType;

  User({
    required this.name,
    required this.email,
    required this.accessToken,
    required this.tokenType,
  });

  factory User.fromJson(Map<String, dynamic> json) => User(
        name: json["name"],
        email: json["email"],
        accessToken: json["accessToken"],
        tokenType: json["token_type"],
      );

  Map<String, dynamic> toJson() => {
        "name": name,
        "email": email,
        "accessToken": accessToken,
        "token_type": tokenType,
      };
}
```

Lalu saya menyimpan datanya seperti ini:

```dart
User user = User(name:'indra',email:'in@in.com',accessToken:'123',tokenType:'bearer');
final storage = Get.find<StorageService>();
storage.saveData('user', user);
```

Kemudian untuk mengambil kembali datanya:

```dart
User user = storage.readData('user');
```

Awalnya semua berjalan dengan baik. Namun setelah saya melakukan _hot restart_ atau _compile_ ulang aplikasi, objek `user` tidak lagi bertipe `User`, melainkan berubah menjadi `Map<String, dynamic>`. Hal ini membuat kode saya yang memanggil properti seperti `user.name` mengalami error.

## Kenapa tipe data nya berubah?

Awalnya saya mengira ini adalah _bug_. Namun setelah saya pelajari lebih lanjut, ternyata saya belum terlalu memahami cara kerja `GetStorage`.

## Di Mana Letak Masalahnya?

Setelah ditelusuri, saya menemukan bahwa **GetStorage** hanya menyimpan data dalam bentuk dasar seperti `Map`, `String`, `int`, `List`, dan sebagainya. Jadi ketika saya menyimpan objek `User`, yang sebenarnya disimpan hanyalah representasi JSON-nya (yakni `Map<String, dynamic>`). Nah, saat dibaca ulang, **GetStorage** tidak secara otomatis mengubahnya kembali ke objek `User`.

## Solusinya: Deserialisasi Manual

Untuk mengatasinya, saya perlu melakukan **konversi manual** saat membaca data:

```dart
final data = storage.readData('user');
User user = User.fromJson(data);
```

Namun saya ingin tetap menggunakan pendekatan `generic <T>` agar kode lebih fleksibel dan dapat digunakan pada berbagai model. Maka, saya ubah method `readData` menjadi seperti ini:

```dart
T? readData<T>(String key, T Function(Map<String, dynamic>)? fromJson) {
  final data = box.read(key);
  if (data == null) return null;

  if (fromJson != null && data is Map<String, dynamic>) {
    return fromJson(data);
  }

  return data as T;
}
```

Sekian catatan dari pengalaman saya ini, semoga bisa membantu yang mengalami masalah serupa.
