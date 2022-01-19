---
title: "Menghapus Partisi Linux Dualboot"
date: 2022-01-19T10:31:00+08:00 # example 2020-01-01T24:00:00+0800. Date should be the current time! you need insert it manually.
description: "Menghapus partisi linux pada mesin dualboot dengan windows"
tags:
- dual boot
categories:
- troubleshooting
---

Dulu saya untuk menghapus partisi Linux dualboot dengan partisi Windows selalu install ulang keseluruhan mesinnya. Dan itu membutuhkan waktu yang lama lagi untuk menginstall Windows, update driver dan yang lain-lainnya. Yang dibutuhin kan hanya menghapus partisi Linux, kenapa harus install ulang keseluruhan mesinnya kan?

Dan setelah saya cari-cari ternyata ada cara yang mudahnya tanpa harus install ulang keseluruhan mesinnya. Jadi kita hanya menghapus partisi Linuxnya aja daripada juga ikut menghapus partisi Windows.

Dan tulisan ini hanya catatan saya pribadi aja, karena saya orangnya suka oprek-oprek dan coba-coba operasi sistem Linux (haha).

Di mesin saya ini, saya mempunyai dua partisi sistem operasi, yang pertama adalah Windows dan yang kedua adalah Linux distro Kali Linux.

##### Langkah-langkah

Yang pertama kita menghapus partisi Linux pada menu Disk Management Windows. Bisa dilihat di bawah banyak partisi yang ada. Partisi `C:` adalah partisi untuk Windows. Partisi Linux saya berada di `119.05GB` karena saat instalasi Linux saya memberikan ukuran partisi sebesar `120GB`.

![1](https://i.ibb.co/X2xWrs2/1.png)

Setelah kita mengetahui partisinya dimana lalu kita hapus partisinya dengan klik kanan dan pilih `Delete Volume`.

![2](https://i.ibb.co/X8vvb31/2.png)

Setelah itu partisinya yang tadi dihapus akan berubah menjadi unallocated, sekarang extend partisi yang unallocated tersebut ke partisi Windows `C:` atau Data `D:`. Untuk sekarang saya ingin menggabungkan partisi unallocated ke partisi `D:`. Caranya klik kanan pada partisi `D:` dan klik `Extend Volume` untuk menggabungkan partisi unallocated ke partisi yang sudah dipilih.

![3](https://i.ibb.co/fdJ74gS/3.png)

![4](https://i.ibb.co/pyt5LbT/4.png)

Akan muncul menu dialog baru, disitu kita bisa klik Next aja sampai selesai dan Klik Finish. Nanti proses penggabungan partisi akan diproses.

![5](https://i.ibb.co/wzghcg2/5.png)

![6](https://i.ibb.co/cL716y6/6.png)

![7](https://i.ibb.co/mvGp22Y/7.png)

Bisa dilihat di atas, partisi yang unallocated tadi akan tergabung ke partisi `D:`. Dan selanjutnya kita akan menghapus partisi GRUB.

##### Menghapus Partisi GRUB

GRUB adalah tampilan awal pertama ketika kita menyalakan mesinnya lalu memilih operasi sistem apa yang ingin kita gunakan. GRUB itu tersimpan di partisi EFI di Windows. Lalu bagaimana kita akses partisi EFI? Karena partisi EFI tidak bisa langsung akses begitu saja. Cara untuk melihat partisi EFI kita bisa menggunakan Powershell atau Command Prompt di Windows.

Disini saya menggunakan Powershell, oh ya jangan lupa ketika mau running Powershell harus `Run As Administrator`. Jika sudah terbuka ketikkan perintah `diskpart` lalu `list volume`. Apa itu volume? Simpelnya itu adalah list volume yang ada di mesin kita.

![8](https://i.ibb.co/rF5m7y7/8.png)

Dari gambar di atas terlihat ada banyak list volume. `Volume 0` adalah DVD-ROM dari laptop saya, `Volume 1` adalah partisi C atau partisi instalasi Windows, `Volume 2` adalah partisi Local Disk D saya. Nah yang perlu diperhatikan disini adalah partisi dengan File System `FAT32` terletak di `Volume 3` itu adalah partisi EFInya.

Setelah kita mengetahui partisi EFInya, kita select volume tersebut dengan perintah `select vol 3`. Saya pilih partisi 3 karena itu adalah posisi partisi EFInya. Untuk beberapa mesin mungkin berbeda untuk letak partisi EFInya, tinggal disesuaikan aja.

![9](https://i.ibb.co/rkFB38B/9.png)

Setelah terpilih saatnya assign letter partisinya supaya kita bisa mengakses partisinya di Windows Explorer. Dengan mengetik perintah `assign letter=x`. Untuk letternya bebas mau pakai abjad g, h, j dan lain-lain, tapi saya akan menggunakan letter `x`.

![10](https://i.ibb.co/QbhrrPs/10.png)

Oke selesai, setelah itu buka Windows Explorer maka akan terlihat partisi tadi yang sudah kita assign dengan letter `x`.

![11](https://i.ibb.co/gTqCrzY/11.png)

Tapi masalahnya ketika double klik pada partisi `X` kita akan mendapatkan error permission seperti gambar di bawah ini.

![12](https://i.ibb.co/nPWvNhb/12.png)

![13](https://i.ibb.co/yXcVnG1/13.png)

Lalu bagaimana cara untuk membukanya? Kita bisa mengakalinya lewat Task Manager.

Pertama buka Task Manager dan pilih `Run new task`.

![14](https://i.ibb.co/1rF3jHP/14.png)

Lalu klik browse dan pilih Local Disk X, disana akan terlihat folder `EFI`.

![15](https://i.ibb.co/mScKK6x/15.png)

![16](https://i.ibb.co/xGkTF3L/16.png)

Double klik pada folder `EFI`, isinya ada folder `Boot`, `kali` dan `Microsoft`.

![17](https://i.ibb.co/M8pt5sv/17.png)

Lalu hapus folder `kali` karena kebetulan saya menggunakan distro Kali Linux. Setiap distro memiliki nama folder yang berbeda, contohnya jika menggunakan distro Manjaro foldernya akan bernama `Manjaro`.

![18](https://i.ibb.co/ZgsCypG/18.png)

![19](https://i.ibb.co/NmHMpMn/19.png)

Dan selesai deh. Oh ya jangan lupa untuk menghapus letter yang sudah diassign tadi di Powershell dengan perintah `remove letter=x` dan klik Enter.

![20](https://i.ibb.co/0DNsvPM/20.png)

Lalu kita cek di Windows Explorer maka partisi `X` tadi akan hilang.

![21](https://i.ibb.co/jrHpPZ7/21.png)

Dan semuanya selesai. Sekarang kita bisa restart mesin kita dan masuk kembali ke Windows.