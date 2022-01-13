---
author: "Muhammad Rizki Maulidan"
title: "Laravel .env Information Disclosure"
date: 2022-01-02T16:38:00+08:00
description: "Laravel .env Information Disclosure"
tags:
- misconfiguration
- laravel
categories:
- hacking
---

Sebenarnya ini bukan bug atau vulnerability, melainkan hanya misconfiguration. Jadi information disclosure itu bocornya informasi ketika website tidak sengaja mengungkapkan informasi yang sensitif kepada pengguna.

Ya memang informasi yang bocor itu dianggap tidak berguna oleh pengguna biasa, tapi untuk seseorang Bug Hunter menurut mereka itu hal yang sangat luar biasa. Kenapa? itu salah satu upaya untuk menggali informasi yang lebih dalam lagi dari website tersebut. Di file `.env` Laravel terdapat banyak informasi yang sensitif, seperti `APP_KEY`, informasi database, `AWS_KEY` dll.

Dan yang bikin kaget, ketika saya melakukan testing ke beberapa website pemerintahan banyak yang kena misconfiguration ini, entah karena si developer lupa setting file `.env` ketika di production atau mungkin ada hal yang lain.

##### Step of procedure:

Cari website target, biasanya kita bisa melakukan testing misconfiguration ini pada url login Laravel biasanya terletak di website.com/`login`.

Lalu ubah input name menjadi array pada tab inspect element.

![Image 1](https://i.imgur.com/hzw6SB5.png)

Login menggunakan kredensial random lalu klik login.

![Image 2](https://i.imgur.com/EHlPWGG.png)

Nah nanti tampilannya berubah menjadi error seperti di bawah ini.

![Image 3](https://i.imgur.com/e4LRMw1.png)

Scroll aja ke bawah sampai ketemu variabel environment.

![Image 4](https://i.imgur.com/Esx7kry.png)

Jadi akan terlihat mulai dari `APP_KEY`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD` dll. Setelah itu apa yang kita lakukan? Ya mungkin bisa mencoba akses ke database tersebut dengan remote mysql (jika port 3306 open) dan melakukan upload shell backdoor menggunakan into outfile method atau yang lainnya.

