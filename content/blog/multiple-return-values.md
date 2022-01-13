---
title: "Multiple Return Value"
date: 2021-11-10T21:20:00+08:00
description: "Multiple return values pada bahasa pemrograman Golang"
tags:
- golang
categories:
- programming
---

Oke artikel kali ini kita akan membahas apa itu konsep multiple return value.

Saya menemukan konsep ini pada bahasa pemrograman Go, tapi setelah saya mencoba-coba bahasa Python ternyata mereka juga ada konsep multiple return values. Fitur ini ternyata sangat powerful di beberapa case.

Simpelnya konsep ini bisa disebut `sebuah function yang bisa return banyak nilai (values)`.

Go memiliki built-in support untuk multiple return values, kadang fitur ini dipakai ketika ingin return hasil dan error dari function.

Untuk mendeklarasikan function biasa seperti contoh kode di bawah ini :

```go
func someFunction() string {
    return "someFunction() called"
}
```

Namun bagaimana jika menggunakan konsep multiple return values? Contohnya seperti kode di bawah ini :

```go
func multipleReturn() (string, string) {
    return "This function return", "multiple values"
}
```

```go
package main

import "fmt"

func someFunction() string {
    return "someFunction() called"
}

func multipleReturn() (string, string) {
    return "This function return", "multiple values"
}

func main() {

    a := someFunc()
    // a : someFunction() called

    b, c := multipleReturn()
    // b : This function return
    // c : multiple values

    _, d := multipleReturn()
    // d : multiple values
}
```

Jika hanya menginginkan subset dari nilai yang dikembalikan, gunakan pengidentifikasi kosong (blank identifier) menggunakan tanda underscore `_`