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

![Image 1](blog/menghapus-partisi-linux-dualboot/1.png)

Setelah kita mengetahui partisinya dimana lalu kita hapus partisinya dengan klik kanan dan pilih `Delete Volume`.

![Image 2](blog/menghapus-partisi-linux-dualboot/2.png)

Setelah itu partisinya yang tadi dihapus akan berubah menjadi unallocated, sekarang extend partisi yang unallocated tersebut ke partisi Windows `C:` atau Data `D:`. Untuk sekarang saya ingin menggabungkan partisi unallocated ke partisi `D:`. Caranya klik kanan pada partisi `D:` dan klik `Extend Volume` untuk menggabungkan partisi unallocated ke partisi yang sudah dipilih.

![Image 3](blog/menghapus-partisi-linux-dualboot/3.png)

![Image 4](blog/menghapus-partisi-linux-dualboot/4.png)

Akan muncul menu dialog baru, disitu kita bisa klik Next aja sampai selesai dan Klik Finish. Nanti proses penggabungan partisi akan diproses.

![Image 5](blog/menghapus-partisi-linux-dualboot/5.png)

![Image 6](blog/menghapus-partisi-linux-dualboot/6.png)

![Image 7](blog/menghapus-partisi-linux-dualboot/7.png)

Bisa dilihat di atas, partisi yang unallocated tadi akan tergabung ke partisi `D:`. Dan selanjutnya kita akan menghapus partisi GRUB.

##### Menghapus Partisi GRUB

GRUB adalah tampilan awal pertama ketika kita menyalakan mesinnya lalu memilih operasi sistem apa yang ingin kita gunakan. GRUB itu tersimpan di partisi EFI di Windows. Lalu bagaimana kita akses partisi EFI? Karena partisi EFI tidak bisa langsung akses begitu saja. Cara untuk melihat partisi EFI kita bisa menggunakan Powershell atau Command Prompt di Windows.

Disini saya menggunakan Powershell, oh ya jangan lupa ketika mau running Powershell harus `Run As Administrator`. Jika sudah terbuka ketikkan perintah `diskpart` lalu `list volume`. Apa itu volume? Simpelnya itu adalah list volume yang ada di mesin kita.

![Image 8](blog/menghapus-partisi-linux-dualboot/8.png)

Dari gambar di atas terlihat ada banyak list volume. `Volume 0` adalah DVD-ROM dari laptop saya, `Volume 1` adalah partisi C atau partisi instalasi Windows, `Volume 2` adalah partisi Local Disk D saya. Nah yang perlu diperhatikan disini adalah partisi dengan File System `FAT32` terletak di `Volume 3` itu adalah partisi EFInya.

Setelah kita mengetahui partisi EFInya, kita select volume tersebut dengan perintah `select vol 3`. Saya pilih partisi 3 karena itu adalah posisi partisi EFInya. Untuk beberapa mesin mungkin berbeda untuk letak partisi EFInya, tinggal disesuaikan aja.

![Image 9](blog/menghapus-partisi-linux-dualboot/9.png)

Setelah terpilih saatnya assign letter partisinya supaya kita bisa mengakses partisinya di Windows Explorer. Dengan mengetik perintah `assign letter=x`. Untuk letternya bebas mau pakai abjad g, h, j dan lain-lain, tapi saya akan menggunakan letter `x`.

![Image 10](blog/menghapus-partisi-linux-dualboot/10.png)

Oke selesai, setelah itu buka Windows Explorer maka akan terlihat partisi tadi yang sudah kita assign dengan letter `x`.

![Image 11](blog/menghapus-partisi-linux-dualboot/11.png)

Tapi masalahnya ketika double klik pada partisi `X` kita akan mendapatkan error permission seperti gambar di bawah ini.

![Image 12](blog/menghapus-partisi-linux-dualboot/12.png)

![Image 13](blog/menghapus-partisi-linux-dualboot/13.png)

Lalu bagaimana cara untuk membukanya? Kita bisa mengakalinya lewat Task Manager.

Pertama buka Task Manager dan pilih `Run new task`.

![Image 14](blog/menghapus-partisi-linux-dualboot/14.png)

Lalu klik browse dan pilih Local Disk X, disana akan terlihat folder `EFI`.

![Image 15](blog/menghapus-partisi-linux-dualboot/15.png)

![Image 16](blog/menghapus-partisi-linux-dualboot/16.png)

Double klik pada folder `EFI`, isinya ada folder `Boot`, `kali` dan `Microsoft`.

![Image 17](blog/menghapus-partisi-linux-dualboot/17.png)

Lalu hapus folder `kali` karena kebetulan saya menggunakan distro Kali Linux. Setiap distro memiliki nama folder yang berbeda, contohnya jika menggunakan distro Manjaro foldernya akan bernama `Manjaro`.

![Image 18](blog/menghapus-partisi-linux-dualboot/18.png)

![Image 19](blog/menghapus-partisi-linux-dualboot/19.png)

Dan selesai deh. Oh ya jangan lupa untuk menghapus letter yang sudah diassign tadi di Powershell dengan perintah `remove letter=x` dan klik Enter.

![Image 20](blog/menghapus-partisi-linux-dualboot/20.png)

Lalu kita cek di Windows Explorer maka partisi `X` tadi akan hilang.

![Image 21](blog/menghapus-partisi-linux-dualboot/21.png)

Dan semuanya selesai. Sekarang kita bisa restart mesin kita dan masuk kembali ke Windows.