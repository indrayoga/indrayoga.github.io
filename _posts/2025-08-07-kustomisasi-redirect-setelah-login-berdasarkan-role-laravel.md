---
layout: post
title: Kustomisasi Redirect Setelah Login Berdasarkan Role di Laravel
subtitle: Membuat dua atau lebih redirect login berdasarkan role yang ada
comments: true
tags: [laravel, pemrograman]
cover-img: /assets/img/kustomisasi-redirect-setelah-login-berdasarkan-role.png
thumbnail-img: /assets/img/kustomisasi-redirect-setelah-login-berdasarkan-role.png
share-img: /assets/img/kustomisasi-redirect-setelah-login-berdasarkan-role.png
---

**Bismillahirrahmannirrahim.**

# Kustomisasi Redirect Setelah Login Berdasarkan Role

> **Masalah:**  
> Ketika user **superadmin** telah login dan mengakses base URL (misal: `http://ekin.dev/`), muncul pesan **403 Forbidden**. Hal ini terjadi karena halaman redirect ke `/dashboard`, padahal seharusnya ke `/admin/dashboard`.

---

## Contoh Kasus

Aplikasi **Ekinerja** memiliki beberapa role dengan halaman dashboard yang berbeda:

| Role       | Halaman Dashboard  |
| ---------- | ------------------ |
| Pegawai    | `/dashboard`       |
| Superadmin | `/admin/dashboard` |

Secara default, Laravel akan me-redirect user ke `/dashboard` atau `/home` setelah login. Untuk menyesuaikan redirect berdasarkan role, Anda perlu melakukan kustomisasi.

---

## Solusi

Pada **Laravel 11**, gunakan method `$middleware->redirectUsersTo` untuk menentukan redirect berdasarkan role user.

### **Langkah-langkah:**

1. **Buka file:**  
   `bootstrap/app.php`

2. **Tambahkan kode berikut:**

   ```php
   $middleware->redirectUsersTo(function ($request) {
        if ($request->user()->role->value == RoleUser::SUPERADMIN->value) {
             return route('admin.dashboard', absolute: false);
        }
        return route('dashboard', absolute: false);
   });
   ```

   > **Catatan:**  
   > Pastikan enum `RoleUser` dan relasi `role` pada user sudah sesuai dengan struktur aplikasi Anda.

---

## 📚 Referensi

- [Customizing Auth Middlewares in Laravel 11 – daryllegion.com](https://daryllegion.com/customizing-auth-middlewares-in-laravel-11)

---
