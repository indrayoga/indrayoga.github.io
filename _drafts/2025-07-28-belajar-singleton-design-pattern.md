Design Pattern Pertama: Singleton

Design pattern pertama yang saya pelajari adalah Singleton. Menurut saya, pola desain ini adalah salah satu yang paling sering digunakan dalam pengembangan perangkat lunak. Mengapa demikian? Karena salah satu implementasi paling umum dari Singleton adalah pada saat kita menulis kode untuk koneksi ke database.

Apa itu Singleton?

Singleton adalah pola desain yang memastikan bahwa sebuah kelas hanya memiliki satu instance saja selama siklus hidup aplikasi, dan menyediakan cara global untuk mengakses instance tersebut.
Sumber referensi: Refactoring.Guru - Singleton Pattern
Kenapa Harus Singleton?

Untuk memudahkan pemahaman, mari kita analogikan dengan printer di kantor. Tidak mungkin setiap karyawan diberi satu printer pribadi — cukup satu printer saja yang bisa digunakan bersama. Semua orang tetap bisa mencetak dokumen, tapi yang digunakan hanya satu printer secara bersama-sama.

Begitu juga dalam pemrograman. Misalnya saat membuat koneksi ke database, tanpa pola Singleton, kita bisa saja membuat koneksi baru setiap kali dibutuhkan. Padahal, dalam banyak kasus, satu koneksi yang dikelola dengan baik sudah cukup dan lebih efisien. Dengan menerapkan Singleton, kita bisa memastikan hanya ada satu koneksi yang digunakan bersama oleh seluruh bagian aplikasi, sehingga lebih hemat sumber daya dan lebih stabil.
