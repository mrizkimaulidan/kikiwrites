---
title: "Customize C++ Coding Style di Vscode"
date: 2022-03-17T22:15:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Merubah coding style c++ di visual studio code"
tags:
- c++
categories:
- programming
---

Jadi saya ngoding C++ tuh pakai software yang namanya Dev-C++, terus kepengen aja pindah dari Dev-C++ ke Visual Studio Code. Tapi untuk ekstensi C++ di Visual Studio Code formatting stylenya saya kurang suka.

Defaultnya dia menggunakan clang formatting standar Visual Studio. Dan setelah saya cari-cari ternyata ada banyak style formatting yang bisa digunakan.

Beberapa list formatting yang bisa digunakan:

- LLVM A style complying with the [LLVM coding standards](https://llvm.org/docs/CodingStandards.html)
- Google A style complying with [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html)
- Chromium A style complying with [Chromium's style guide](https://www.chromium.org/developers/coding-style/)
- Mozilla A style complying with [Mozilla's style guide](https://firefox-source-docs.mozilla.org/code-quality/coding-style/coding_style_cpp.html)
- WebKit A style complying with [Webkit's style guide](https://webkit.org/code-style-guidelines/)
- Microsoft A style complying with [Microsoft's style guide](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options?view=vs-2017)

Bisa lihat untuk masing-masing formatting style link di atas yang sudah saya cantumkan, saya pribadi menyukai coding style dari Google. Bukan berarti saya fanboy Google yaaa (wkwk), tapi karena kebetulan saya pindahan dari bahasa Go yang formattingnya kurang lebih sama seperti bahasa Go, jadi ya pilih coding style dari Google.

Tapi ya kembali lagi sesuai preferensi dan selera masing-masing yaa hehe.

Dan balik lagi gimana kan cara mengubah coding stylenya? Oke simak step di bawah:

Wajib install ekstensi C++ di Visual Studio Code.

![Image 1](blog/customize-c-coding-style-di-vscode/1.png)

Nah kalau udah tinggal buka setting di Visual Studio Code dan ketik "clang format" dan pilih `C_Cpp: Clang_format_fallback Style` defaultnya terisi Visual Studio.

![Image 2](blog/customize-c-coding-style-di-vscode/2.png)

Nah valuenya tinggal diubah aja, karena saya menggunakan coding style dari Google maka saya isi `{ BasedOnStyle: Google, IndentWidth: 4, ColumnLimit: 0}`

![Image 3](blog/customize-c-coding-style-di-vscode/3.png)

Oh iya kalau mau diubah menjadi coding style yang lain bisa diubah aja sesuai list formatting style yang saya cantumkan tadi. Kalau mau menggunakan LLVM bisa diubah menjadi `{ BasedOnStyle: LLVM, IndentWidth: 4, ColumnLimit: 0}`.

Kalau mau diubah menjadi Chromium style bisa diubah menjadi `{ BasedOnStyle: Chromium, IndentWidth: 4, ColumnLimit: 0}` dan seterusnya.

Referensi:
- https://code.visualstudio.com/docs/getstarted/settings