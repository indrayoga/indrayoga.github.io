---
layout: post
title: "Perintah Command Line Untuk SysAdmin Linux #3 - mencari file yang mengandung script berbahaya dengan `grep`"
subtitle: Berikut ini beberapa perintah command line di server linux yang saya pelajari dan biasa saya gunakan selama menjadi system administrator
author: Indra Yoga
comments: true
tags: [linux, ubuntu]
---

**Bismillahirrahmannirrahim.**

Ketika sistem sudah berhasil di susupi, biasa pelaku melanjutkan dengan menanamkan `web shell` yang bisa digunakan oleh pelaku untuk menjadi backdoor agar dapat mengakses sistem kembali, `web shell` merupakan script yang mampu mengeksekusi perintah shell pada `web server`, `web shell` bisa bermacam macam mulai dengan yang sederhana atau yang sudah di `obfuscated`, `web shell` biasanya disimpan di tempat yang jarang atau yang biasanya developer tidak pernah meletakkan script file di direktori tersebut, seperti di direktori images dan semacamnya.

Tapi, kita bisa melakukan scanning di direktori aplikasi kita untuk mencari tau file itu mengandung malicious script, yaitu dengan perintah `grep`.

```
grep -RPn "(passthru|shell_exec|system|phpinfo|base64_decode|chmod|mkdir|fopen|fclose|fclose|readfile) *(" nama_direktori
```

Perintah tersebut akan menampilkan daftar daftar file beserta alamatnya yang mengandung script
`passthru|shell_exec|system|phpinfo|base64_decode|chmod|mkdir|fopen|fclose|fclose|readfile` kita bisa menambahkan kata kunci yang kira kira ada di `web shell`, tentu hasil dari perintah tersebut tidak langsung membuat file tesebut adalah `malicious file` atau `web shell` perlu di teliti lagi pada file tersebut.
