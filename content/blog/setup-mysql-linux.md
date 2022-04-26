---
title: "Setup MySQL Linux"
date: 2022-04-27T01:04:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Reset password mysql root"
tags:
- mysql
categories:
- troubleshooting
---

Artikel singkat untuk setup MySQL di Linux. Kebetulan gua lagi di Kali Linux. Untuk setup MySQL masih perlu untuk konfigurasi passwordnya. Karena secara default kita ga bisa masuk sebagai root karena passwordnya belum diketahui.

Jadi di artikel ini akan mereset password untuk user `root` pada database.

Install MySQL server menggunakan APT :

```bash
$ sudo apt install mysql-common
```

Lalu masuk ke MySQL menggunakan privilege sudo :

```bash
$ sudo mysql
```

Lalu menu MySQL di terminal akan terbuka :

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 6
Server version: 10.6.7-MariaDB-3 Debian buildd-unstable

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

Masukkan perintah `ALTER USER 'root'@'localhost' IDENTIFIED BY 'password_yang_diinginkan';` :

Fungsi diatas akan merubah password untuk username root. Dan sesuaikan dengan password yang kalian mau.

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 6
Server version: 10.6.7-MariaDB-3 Debian buildd-unstable

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> `ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';`
```

Karena gua pengen passwordnya sama seperti nama user maka gua masukin `root` sebagai passwordnya. Jadi nanti kalau mau login ke MySQL tinggal masukkin username root dan passwordnya juga root.

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 6
Server version: 10.6.7-MariaDB-3 Debian buildd-unstable

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
Query OK, 0 rows affected (0.075 sec)

MariaDB [(none)]> 
```

Oke sudah selesai, tinggal exit terminal MySQLnya dan login dengan akun root :

```bash
$ mysql -uroot -proot

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 7
Server version: 10.6.7-MariaDB-3 Debian buildd-unstable

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

Okesip berhasil deh.