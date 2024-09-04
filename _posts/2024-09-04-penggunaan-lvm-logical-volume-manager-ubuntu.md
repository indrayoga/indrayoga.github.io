---
title: Bagaimana Penggunaan LVM Logical Volum Manager di Ubuntu 
description: Berikut cara cara penggunaan dan pengaturan LVM, Seperti menambah kapasitas, membuat Logical Volume baru.
author: Indra Yoga Permana
date: 2024-09-04 12:00:00 +0800
pin: false
comments: true
categories: [Blog]
tags: [server,ubuntu]
---

### Memeriksa ukuran, ruang yang kosong atau terisi
```
sudo df -h
```

![output dari perintah df-h](/images/2024-09-04-penggunaan-lvm-logical-volume-manager-ubuntu-image.png)

dari gambar diatas kita dapat ketahui, saya memiliki dua logical volume yaitu `/dev/mapper/ubuntu--vg-ubuntu--lv` yang dimount ke `/` dan `/dev/mapper/ubuntu--vg-lv--0` yang di mount ke `/home`.

### Memeriksa ruang kosong di volume group
```
sudo vgdisplay
```
![output dari perintah vgdisplay](/images/2024-09-04-penggunaan-lvm-logical-volume-manager-ubuntu-image-1.png)
dari gambar diatas keta ketahui ada 1 volume group dengan nama ubuntu-vg dan ruang kosong (yang di kotak merah) 6.50 GB.

### Menambah(extend) kapasitas Logical Volume (LV) dari ruang kosong yang tersedia di Volume Group

Misal kita ingin menambahkan ruang kosong yang tersedia sekian GB ke *logical volume* `/dev/mapper/ubuntu--vg-ubuntu--lv`
```
sudo lvextend -L +2G /dev/mapper/ubuntu--vg-ubuntu--lv
```
Jika ingin menambahkan seluruh ruang kosong tersedia ke LV
```
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

setelah itu, langkah selanjutnya jalankan perintah ```resize2fs```
```
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

kita lihat lagi kapasitas ruang kita dengan `df -h`
![output dari perintah df -h](/images/2024-09-04-penggunaan-lvm-logical-volume-manager-ubuntu-image-2.png)

### Membuat Logical Volume (LV) baru dari ruang kosong yang tersedia di Volume Group dan mount ke filesystem
```
sudo lvcreate -l +100%FREE -n ubuntu--vg-lv--1 ubuntu-vg
```
Perintah tersebut akan membuat *logical volume* baru dengan nama `ubuntu--vg-lv--1` di dalam *volume group* `ubuntu-vg`

kemudian *logical volume* tersebut kita *mount* ke *filesystem*, misal kita ingin *logical volume* di mount ke `/var`

Sebelumnya kita buat *filesystem* nya.
```
sudo mkfs.ext4 /dev/ubuntu-vg/ubuntu--vg-lv--1
```

kemudian kita *mount* ke `/var`
```
sudo mount /dev/ubuntu-vg/ubuntu--vg-lv--1 /var
```

jika ingin membuat mount persistent untuk memastikan filesystem di mount otomatis ketika reboot, edit file `/etc/fstab`

sebelumnya kita ambil dulu UUID dengan perintah `blkid`:
```
sudo blkid /dev/ubuntu-vg/ubuntu--vg-lv--1
```
kemudian edit file `/etc/fstab` dan tambahkan baris ini
```
UUID=isi UUID disini /var ext4 defaults 0 2
```
