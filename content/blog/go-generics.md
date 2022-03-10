---
title: "Go Generics"
date: 2022-03-10T11:26:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Mengenali generics pada Go"
tags:
- go
- golang
categories:
- programming
---

Go akan memiliki generics pada versi `1.18`. Sebelum artikel ini dirilis versi tersebut masih belum official release dan masih beta. Tapi kita tetap bisa menggunakan versi tersebut. Dan untuk menggunakan generics diharuskan untuk menginstall versi Go `1.18` bisa pergi ke website official di sini https://go.dev/dl/.

Go umumnya diketahui sebagai bahasa pemrograman yang statically-typed programming, artinya setiap pendeklarasian variabel harus ditentukan tipe datanya apa dan tidak dapat bisa diubah tipe datanya ketika program dijalankan. Berbeda halnya dengan dynamic programming seperti PHP, Python dan yang lain-lainnya yang menggunakan dynamic programming.

Dan sekarang apa itu generics? Misalkan ada contoh sebuah function, sebutlah function untuk melakukan operasi pertambahan di matematika, yang dimana di parameter menerima dua tipe data anggap saja `int64` dan `float64` dan masing masing akan mengembalikan nilai `int64` dan `float64`. Nah jika menggunakan generics kita hanya bisa menuliskan satu buah function saja untuk melakukan itu.

Berbeda jika tidak menggunakan generics, dimana kita harus mengimplementasikan dua buah function yang memiliki parameter yang berbeda dan return type yang berbeda juga.

Oke mungkin akan sedikit membingungkan, kita lanjut saja ke prakteknya sesuai dengan kasus yang sudah saya berikan di paragraf 3 dan 4.

Yang pertama kita akan menyelesaikan masalah di paragraf 4 tanpa generics dimana akan ada dua buah function yang melakukan operasi pertambahan di matematika dengan tipe data `int64` dan `float64`.
```go
package main

import "fmt"

// Melakukan operasi pertambahan di matematika dengan
// parameter int64.
// Return type int64.
func SumInts(numbers []int64) int64 {
    var sum int64

    for _, number := range numbers {
        sum = sum + number
    }

    return sum
}}

// Melakukan operasi pertambahan di matematika dengan
// parameter float64.
// Return type float64.
func SumFloats(numbers []float64) float64 {
    var sum float64

    for _, number := range numbers {
        sum = sum + number
    }

    return sum
}

func main() {
    numbers64 := []int64{1, 2, 3, 4, 5}
    floats64 := []float64{1.1, 1.2, 1.3, 1.4, 1.5}

    fmt.Println("Sum ints :", SumInts(numbers64))
    fmt.Println("Sum floats :", SumFloats(floats64))
}
```

```bash
$ go run main.go
Sum ints : 15
Sum floats : 6.5
```

Jika dijalankan di terminal outputnya akan seperti di atas.

Dan sekarang kita akan menggunakan generics melakukan operasi yang sama pada kode script di atas tapi hanya akan membutuhkan satu buah function saja.

```go
package main

import "fmt"

// Melakukan operasi pertambahan di matematika dengan
// parameter int64 atau float64.
// Return type akan menyesesuaikan sesuai
// tipe data apa yang dimasukkan pada parameter.
func SumIntsOrFloatsWithGenerics[T int64 | float64](numbers []T) T {
    var sum T

    for _, number := range numbers {
        sum = sum + number
    }

    return sum
}

func main() {
    numbers64 := []int64{1, 2, 3, 4, 5}
    floats64 := []float64{1.1, 1.2, 1.3, 1.4, 1.5}

    ints64Generics := SumIntsOrFloatsWithGenerics(numbers64) // return int64
    floats64Generics := SumIntsOrFloatsWithGenerics(floats64) // return float64

    fmt.Println("Sum ints or floats with generics :", ints64Generics)
    fmt.Println("Sum ints or floats with generics :", floats64Generics)
}
```

```bash
$ go1.18rc1 run main.go
Sum ints or floats with generics: 15
Sum ints or floats with generics : 6.5
```

Bisa dilihat jika menggunakan generics hanya membutuhkan satu buah function yang memiliki return type yang berbeda sesuai dengan comparable type.

Penjelasan sintaks:

Penulisan umum generics

```go
func funcName[dataType <ComparableType>](params)
```

```go
func SumIntsOrFloatsWithGenerics[T int64 | float64 ](numbers []T) T {
```

Huruf `T` diatas akan kompatibel dengan tipe data `int64` atau `float64`.

`(numbers []T)` adalah parameternya.

Slice `T` atau `[]T` akan kompatibel menyesesuaikan dengan comparable data typenya yaitu `int64` atau `float64`. Dan juga return type `T` akan ikut menyesesuaikan juga dengan comparable data typenya `int64` atau `float64`.

Intinya jika di parameter function tersebut diisi dengan tipe data `int64` maka function tersebut akan memproses menggunakan tipe data `int64`.

Sebaliknya jika di parameter function tersebut diisi dengan tipe data `float64` maka function tersebut akan memproses menggunakan tipe data `float64`.

Jika memiliki banyak comparable type maka akan membuat parameter function memanjang kan? Disini bisa menggunakan `Generic Type Contraint`. Bisa menggunakan interface. Sebagai contoh di bawah:

```go
type Number interface{
    int32 | int64 | float32 | float64
}

func SumIntsOrFloatsWithGenerics[T Number](numbers []T) T {
    var sum T

    for _, number := range numbers {
        sum = sum + number
    }

    return sum
}
```

Contoh di atas hanyalah contoh kasus sederhana saja. Menurut saya generics sangat bagus di beberapa kasus.

Beberapa sumber yang bisa dilihat:
- https://go.dev/doc/tutorial/generics
- https://www.youtube.com/watch?v=sVCQFV5P7_g