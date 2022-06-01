---
title: "Kali Linux Fresh Install No Internet Connection"
date: 2022-06-01T21:53:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "No internet connection fresh install kali linux"
tags:
- kali linux
categories:
- troubleshoot
---

Kemarin lagi install Kali Linux 2022.2 di laptop gua entah kenapa koneksi internet ga bisa ngeping ke google, padahal udah terkoneksi sama wifi. Ini permasalahan pertama kali gua dapet waktu Kali Linux versi 2022.1 dan masih ketemu di versi 2022.2.

Setelah cari-cari ternyata file `resolv.conf` di folder `/etc` broken symbolic link yang artinya file yang untuk ngelink dari `resolv.conf` ga ada di folder yang mengarah ke file symbolic linknya.

Jadi yaudah mau ga mau harus buat ulang filenya tanpa symbolic link.

## Lets fix it

Hapus file `resolv.conf` yang lama

```bash
$ sudo rm /etc/resolv.conf
```

Buat file baru di folder `etc` dengan nama `resolv.conf`

```bash
$ sudo touch /etc/resolv.conf
```

Terus isiin nameserver IP router di file `resolv.conf`. By the way masing-masing router punya IP yang berbeda-beda ya, kalau gua pake provider internet merah jadi IP nya seperti ini

```bash
$ sudo nano /etc/resolv.conf

// Isiin kalimat di bawah ke file resolv.conf
nameserver 192.168.1.1
```

Terus yaudah save & exit. Setelah itu coba ping ke google atau terserah kemana aja, yang penting untuk ngecek internet connectionnya udah terkonek atau belum.

Referensi :
- https://forums.kali.org/showthread.php?70445-can-t-acces-internet-but-wifi-connection-is-working-fine
- https://en.wikipedia.org/wiki/Resolv.conf
- https://medium.com/@hsahu24/understanding-dns-resolution-and-resolv-conf-d17d1d64471c