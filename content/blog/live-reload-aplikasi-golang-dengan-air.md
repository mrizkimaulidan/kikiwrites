---
title: "Live Reload Aplikasi Golang Dengan Air"
date: 2022-02-04T21:37:00+08:00 # example 2020-01-01T24:00:00+0800. Date should be the current time! you need insert it manually.
description: "Live reload aplikasi golang dengan air"
tags:
- go
- golang
categories:
- programming
---

Jadi ketika saya ngoding pakai Golang untuk ngebuat Rest API Server agak terlalu susah untuk debugging dan ngelakuin development. Saya biasanya debugging menggunakan function `log.Println()` yang akan tampil di terminal program yang sedang berjalan.

Ya sebenarnya ga ada masalah, fine-fine aja. Tapi lama kelamaan eneg juga selalu tekan shortcut `ctrl + c` untuk berhentiin program dan ngelakuin compile ulang jadi binary file terus running binary filenya.

Ya bisa juga sih untuk ketik perintah `go run main.go` tapi kan sama aja setiap ngelakuin perubahan diberhentiin lagi programnya dan ketik ulang perintah `go run main.go` lagi.

Terus iseng nanya ke salah satu grup Discord, terus ada yang nyaranin pakai `air` supaya hot reloading. Jadi air ini command line utility untuk ngebantu development Go.

Mekanisme utilitas ini dia akan watching beberapa folder dan file yang ada di current directory di aplikasi Go nya, setelah itu ngelakuin building dan dilanjutkan dengan ngerunning ulang programnya.

Dia bakal ngerunning ulang kalau dari beberapa folder yang dia watch ada perubahan, biasanya triggernya pas ngesave filenya.

##### Langkah-langkah

Pertama kita harus download utility airnya. Dan air ini juga cross platform, bisa dipake di berbagai macam sistem operasi. Disini saya mempraktekan di sistem operasi Windows, jadi tinggal download file `.exe` di halaman release di sini https://github.com/cosmtrek/air/releases.

![Image 1](https://i.ibb.co/YN293QH/1.png)

Tinggal pilih aja sesuaikan sistem operasi yang sedang kalian pakai. Karena di sini saya pake Windows ya saya download file dengan ekstensi `.exe`.

![Image 2](https://i.ibb.co/8X8jd9x/2.png)

Nah setelah terdownload rename aja filenya, setelah itu bisa masukin ke path environment variables kalau mau pakai secara global. Kalau untuk praktek di sini saya akan langsung aja pindahin ke folder directory aplikasi Go saya.

![Image 3](https://i.ibb.co/4fhqG05/3.png)

Terlihat di foldernya ada file `air.exe` yang sudah saya pindahkan ke folder directory aplikasi Go saya.

Tinggal buka terminal dan ketik aja `air.exe` untuk ngerunning utility nya. Di sini saya pakai Git Bash, jadi untuk ngerunningnya dengan perintah `/air.exe`

![GIF](https://i.ibb.co/KbwBHYW/ezgif-com-gif-maker.gif)

![GIF](https://i.ibb.co/SvNCyMf/ezgif-com-gif-maker.gif)

Nah mekanismenya dia bakal ngebuat folder dengan nama `tmp` isinya itu binary file yang dari file `main.go`. Jadi sekarang bebas deh setiap kita ngelakuin beberapa perubahan di file yang telah diwatch akan otomatis ngebuild dan ngerunning ulang oleh utility air nya.

Jadi kita ga perlu repot untuk ngeberhentiin programnya, ngelakuin compile ulang, dan running ulang binary filenya.