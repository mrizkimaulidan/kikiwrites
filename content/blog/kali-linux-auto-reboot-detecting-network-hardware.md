---
title: "Kali Linux Auto Reboot Detecting Network Hardware"
date: 2021-12-30T23:34:00+08:00
description: "Mengatasi permasalahan Auto Reboot ketika instalasi Kali Linux pada step Detecting Network Hardware"
tags:
- kali linux
categories:
- troubleshooting
---

Jadi ketika proses instalasi Kali Linux dual boot dengan windows terkena error auto reboot ketika dibagian detecting network hardware. Jadi si komputer atau laptop otomatis reboot. Entah karena apa bisa menyebabkan errornya. Dulu sebelum Kali Linux 2021.3 ga ada masalah, tapi kok di versi 2021.4 ini error.

Cara benerinnya ternyata mudah, jadi ketika di bagian proses detecting network hardware kita exit scriptnya :

1. Masuk ke menu instalasi Kali Linux tapi jangan pilih yang graphical install, cukup pilih yang installation with text mode aja.

2. Tekan kombinasi keyboard `ALT + F2` untuk masuk ke mode terminal. kalau kombinasi `ALT + F1` itu bagian GUI untuk instalasinya.

3. Buka file yang berlokasi di `/bin/check-missing-firmware` dengan nano `nano /bin/check-missing-firmware`.

4. Masukan script `exit 0` sebelum `#!/bin/sh`. Fungsi script `exit 0` akan mengskip proses detect networknya, jadi machine ga bakal otomatis reboot ketika proses detect network hardwarenya.

5. Simpan filenya terus exit terminal. Lalu masuk ke menu instalasinya kembali dengan kombinasi keyboard `ALT + F1`.

6. Selesai, sekarang lanjutkan proses instalasinya sampai selesai.