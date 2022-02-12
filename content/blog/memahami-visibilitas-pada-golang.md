---
title: "Memahami Visibilitas Pada Golang"
date: 2022-02-12T19:51:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Memahami visibilitas pada bahasa pemrograman golang"
tags:
- go
- golang
categories:
- programming
---

Di Golang untuk hak akses suatu properti atau method itu ga ada yang namanya `Public`, `Protected` dan `Private`. Yang ada hanyalah `Exported` dan `Unexported`.

Simpelnya Exported itu suatu properti atau method yang bisa dipakai oleh package yang lain, sedangkan Unexported tidak bisa dipakai oleh package yang lain.

Berbeda pada OOP PHP dan Java yang memakai tiga istilah di atas tersebut. Untuk mengatur visibilitas pada bahasa Go bisa menggunakan format capitalized dan non-capitalized.

- **Capitalized identifiers** adalah exported. Pada suatu properti atau method jika huruf pertamanya menggunakan huruf kapital artinya properti atau method itu exported dan bisa dipakai di luar package.

- **Non-capitalized identifiers** adalah unexported. Nah sedangkan format non-capitalized itu kebalikan dari capitalized format. Artinya suatu properti atau method jika huruf pertamanya huruf kecil maka properti atau method tersebut unexported dan tidak bisa dipakai di luar package.

Biasanya visibilitas ini dipakai ketika mendeklarasikan suatu struct, struct field ataupun method. Jadi jika nama sebuah struct berawalan huruf kapital maka itu exported dan bisa dipakai oleh package yang lain. Dan itupun berlaku juga untuk struct field.

Oke sebagai contoh kita buat dua buah file, yang pertama ada `entity.go` dan file yang kedua bernama `main.go`.

```go
// entity.go
package main

type Book struct {
    Title string
    quantity int
}

type person struct {
    //
}

func (b *Book) GetTitle() string {
    return b.Title
}

func (b *Book) getQuantity() int {
    return b.quantity
}
```

Pada kode di atas dideklarasikan sebuah struct bernama `Book` dengan satu struct field yang berawalan huruf kecil yaitu `quantity`.

Lalu ada sebuah struct lagi bernama `person` yang berawalan dengan huruf kecil dan tidak memiliki struct field.

Disini saya menggunakan method receiver untuk mendeklarasikan dua buah method pada struct `Book` yaitu `GetTitle()` dengan huruf awalan kapital dan `getQuantity()` dengan huruf awalan kecil.

```go
// main.go
package main

import "fmt"

type Book struct {
	Title    string
	quantity int
}

type person struct {
	//
}

func (b *Book) GetTitle() string {
	return b.Title
}

func (b *Book) getQuantity() int {
	return b.quantity
}

func main() {
	book1 := &Book{
		Title:    "Book 1",
		quantity: 2,
	}

	fmt.Println("Book 1 :", book1)
	fmt.Println("Book 1 Name :", book1.GetTitle())
	fmt.Println("Book 1 Quantity :", book1.getQuantity())

	var person person
	fmt.Println("Person :", &person)
}
```

![GIF 1](blog/memahami-visibilitas-pada-golang/1.gif)

Well dari kode di atas jika dijalankan dalam satu package yang sama ga ada masalah. Tapi jika dijalankan di file yang berbeda apa yang akan terjadi? Nah kita lihat selanjutnya.

Oke sekarang kita akan pisahin nih ke file yang berbeda sekarang kita buat file lagi bernama `main.go` isinya akan memakai struct `Book` dan `person` dari package entity.

Dan jangan lupa juga ya buat projeknya menjadi module menggunakan command `go mod init visibility`.

```go
// main.go
package main

import (
	"fmt"
	"visibility/entity"
)

func main() {
	book1 := &Book{
		Title:    "Book 1",
		quantity: 2,
	}

	fmt.Println("Book 1 :", book1)
	fmt.Println("Book 1 Name :", book1.GetTitle())
	fmt.Println("Book 1 Quantity :", book1.getQuantity())

	var person person
	fmt.Println("Person :", &person)
}
```

![GIF 2](blog/memahami-visibilitas-pada-golang/2.gif)

Apa yang terjadi jika kode di atas dijalankan? Ketika dijalankan akan muncul error seperti dibawah ini:

```bash
# command-line-arguments
.\main.go:11:3: cannot refer to unexported field 'quantity' in struct literal of type entity.Book
.\main.go:16:40: book1.getQuantity undefined (cannot refer to unexported field or method getQuantity)
.\main.go:18:13: cannot refer to unexported name entity.person
```

Terlihat dari error di atas bahwa unexported struct, struct field maupun method tidak akan bisa dipakai oleh package yang lain.