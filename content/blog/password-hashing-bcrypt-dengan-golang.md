---
title: "Password Hashing (bcrypt) dengan Golang"
date: 2021-11-10T19:37:00+08:00
description: "Menggunakan library crypt/bcrypt untuk implementasi password hashing dengan bahasa pemrograman Golang"
tags:
- golang
categories:
- programming
---

Untuk password hashing menggunakan bcrypt untuk golang bisa menggunakan library : [https://pkg.go.dev/golang.org/x/crypto/bcrypt](https://pkg.go.dev/golang.org/x/crypto/bcrypt)

Kita harus menggunakan perintah `go get`. Perintah `go get` sama saja dengan `git clone` jika kalian familiar dengan Git.

```bash
$ go get golang.org/x/crypto/bcrypt
```

Selanjutnya kita membuat kode seperti gambar di bawah ini
```go
// main.go
package main
import (
    "fmt"
    "golang.org/x/crypto/bcrypt"
)
func HashPassword(password string) (string, error) {
    bytes, err := bcrypt.GenerateFromPassword([]byte(password), 14)
    return string(bytes), err
}
func CheckPasswordHash(password, hash string) bool {
    err := bcrypt.CompareHashAndPassword([]byte(hash), []byte(password))
    return err == nil
}
func main() {
    password := "secret"
    hash, _ := HashPassword(password) // ignore error for the sake of simplicity
    fmt.Println("Password:", password)
    fmt.Println("Hash:    ", hash)
    match := CheckPasswordHash(password, hash)
    fmt.Println("Match:   ", match)
}
```

Dari penjelasan kode di atas adalah :

```go
func HashPassword(password string) (string, error) {
    bytes, err := bcrypt.GenerateFromPassword([]byte(password), 14)
    return string(bytes), err
}
```
Fungsi `HashPassword()` akan menghasilkan password yang berbentuk bcrypt sesuai dari parameter `password`.

```go
func CheckPasswordHash(password, hash string) bool {
    err := bcrypt.CompareHashAndPassword([]byte(hash), []byte(password))
    return err == nil
}
```
Fungsi `CheckPasswordHash()` adalah untuk menyandingkan atau menyamakan sebuah string biasa ke password yang sudah berbentuk enkripsi bcrypt. Parameter `password` adalah string biasa yang akan disandingkan dengan parameter `hash`.

Hasil dari function di atas adalah boolean, yang dimana jika password matching dengan hash maka akan menghasilkan true. Jika password tidak matching dengan hash maka akan menghasilkan false yang berarti password tidak matching.

```bash
$ go run main.go
Password    : secret
Hash        : $2a$14$ajq8Q7fbtFRQvXpdCq7Jcuy.Rx1h/L4J60OtxgyNLbAYctGMJ9tK
Match       : true
```

```bash
$ go run main.go
Password    : wrongpassword
Hash        : $2a$14$ajq8Q7fbtFRQvXpdCq7Jcuy.Rx1h/L4J60OtxgyNLbAYctGMJ9tK
Match       : false
```