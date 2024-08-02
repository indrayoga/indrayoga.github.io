---
title: Mengamankan Server Ubuntu
description: cara cara yang dapat dilakukan untuk mengamankan server.
author: Indra Yoga Permana
date: 2024-07-25 14:35:00 +0800
pin: false
categories: [Blog]
tags: [server,ubuntu,security]
---

**Bismillahirrahmannirrahim.**

Beberapa cara yang biasa saya lakukan untuk mengamankan server/web server ubuntu.

 - Mengaktifkan firewall

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

 - memasang fail2ban
  
    fail2ban adalah aplikasi yang memonitor log sistem kita di server, fail2ban membaca log seperti log ssh, log apache dan lainya dan melakukan analisa dan dapat melakukan `banned` dalam waktu tertentu, untuk instalasi dan konfigurasi bisa di baca [disini](https://www.linode.com/docs/guides/using-fail2ban-to-secure-your-server-a-tutorial/)

 - memasang [ModSecurity](https://www.linode.com/docs/guides/securing-apache2-with-modsecurity/) (Web Application Firewall).
 -  Jika memakai apache, kita bisa memakai [mod_evasive](https://www.howtogeek.com/devops/how-to-configure-mod_evasive-for-apache-ddos-protection/) untuk mencegah DDOS.


