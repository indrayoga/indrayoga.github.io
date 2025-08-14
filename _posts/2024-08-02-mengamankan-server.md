---
layout: post
title: Mengamankan Server Ubuntu
subtitle: cara cara yang dapat dilakukan untuk mengamankan server.
tags: [server, ubuntu, security]
comments: true
mathjax: true
author: Indra Yoga
category: posts
---

**Bismillahirrahmannirrahim.**

Beberapa cara yang biasa saya lakukan untuk mengamankan server/web server ubuntu.

### Mengaktifkan firewall

Untuk mengaktifkan firewall :

```
sudo ufw enable
```

ketika firewall aktif, maka semua port akan ditutup kecuali yang kita bolehkan dibuka, untuk membuka port perintahnya :

```
sudo ufw allow 80
sudo ufw allow 443
```

port `80` dan `443` adalah port untuk `http` dan `https` sehingga aplikasi web kita bisa diakses dari publik.

### Memasang fail2ban

fail2ban adalah aplikasi yang memonitor log sistem kita di server, fail2ban membaca log seperti log ssh, log apache dan lainya dan melakukan analisa dan dapat melakukan `banned` dalam waktu tertentu, untuk instalasi dan konfigurasi bisa di baca [disini](https://www.linode.com/docs/guides/using-fail2ban-to-secure-your-server-a-tutorial/)

### Memasang [ModSecurity](https://www.linode.com/docs/guides/securing-apache2-with-modsecurity/) (Web Application Firewall).

### Memasang Proteksi DDOS

Jika memakai apache, kita bisa memakai [mod_evasive](https://www.howtogeek.com/devops/how-to-configure-mod_evasive-for-apache-ddos-protection/) untuk mencegah DDOS.

### Konfigurasi Hak Akses Direktori dan File Aplikasi Web

Konfigurasi hak akses sangat penting, karena hak akses akan menentukan siapa atau apa yang bisa melihat, mengubah, menghapus atau mengeksekusi file tersebut.
Jangan jadikan www-data atau apache atau user web server lainnya menjadi owner, melainkan jadikan group direktori atau file tersebut ke www-data/apache.

```
sudo chown -R developer:www-data /var/www/project
```

untuk direktori didalam aplikasi web sebaiknya level permission-nya adalah 755.
cara mengubah permission di dalam direktori aplikasi web kita :

```
sudo find /var/www/project -type d -exec chmod 755 {} \;
```

untuk file didalam aplikasi web sebaiknya level permission-nya adalah 644.

```
sudo find /var/www/project -type f -exec chmod 644 {} \;
```

Jika ada suatu kondisi user web server harus memiliki write akses ke suatu direktori maka kita set permission 775 pada direktori tersebut, seperti contoh pada direktori storage dan bootstrap/cache di laravel

```
sudo chmod -R 775 /var/www/project/storage
sudo chmod -R 775 /var/www/project/bootstrap/cache
```

### Mengamankan direktori upload file

biasanya website atau aplikasi berbasis web menyediakan fitur upload, fitur upload ini bisa menjadi celah keamanan untuk penyerang bila tidak di implementasi dengan baik, selain implementasi keamanan di sisi aplikasi yang menyediakan fitur upload file, kita juga bisa menambahkan konfigurasi keamanan tambahan dari sisi server, misal jika kita menggunakan apache, kita bisa membuat whitelist apa saja file yang boleh di akses di direktori upload tersebut, misal kita punya direktori dengan path berikut :

```
/var/www/site.contoh.com/uploads
```

kita bisa membuat file `.htaccess` id directory `uploads` dengan isi seperti berikut :

```
order allow,deny

<Files ~ "\.(pdf)$">
   allow from all
</Files>
```

maksud file `.htaccess` diatas adalah agar client tidak bisa membuka file dengan extensi apapun (misal:php,jsp,js,css dll) kecuali yang hanya berekstensi `.pdf` , jika file `uploads` tersebut isinya untuk gambar,kita sesuaikan seperti berikut :

```
order allow,deny

<Files ~ "\.(jpg|png|jpeg)$">
   allow from all
</Files>
```

jika kita takut penyerang melakukan replace file .htaccess kita bisa lakukan konfigurasi di file apache.conf, tambahkan konfigurasi seperti berikut :

```
<Directory /var/www/site.contoh.com/uploads/>
  order allow,deny

  <Files ~ "\.(jpg|png|jpeg)$">
    allow from all
  </Files>
</Direcoty>
```
