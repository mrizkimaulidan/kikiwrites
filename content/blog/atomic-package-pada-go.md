---
title: "Atomic Package Pada Go"
date: 2022-03-18T19:47:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Mengenal package atomic pada bahasa pemrograman Go"
tags:
- go
- golang
categories:
- programming
---

Kalau semisalnya melakukan increment atau decrement pada variabel menggunakan channel atau sebuah variabel yang digunakan bersamaan pada satu waktu yang sama, goroutines tidak akan sync/synchronized dan akan menghasilkan output yang keliru.

Operasi Atomic biasanya diimplementasikan ke hardware level. Yang artinya kita bisa membuat suatu variabel Atomic yang digunakan bersamaan dengan banyak goroutines untuk merubah valuenya secara benar. Go memiliki package atomic yang membantu sinkronisasi saat melakukan konkurensi.

Package Atomic berada di `sync/atomic`.

Di package Atomic ada banyak function built-in yang bisa digunakan, ada load, store dan operasi penambahan untuk tipe data `int32`, `int64`, `uint32`, `uint64` dan lain-lain. Karena `int` hanya tipe data yang primitif yang bisa disinkronkan secara Atomic.

Sebagai contoh ada kodingan yang tanpa menggunakan Atomic.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var counter int64
	var wg sync.WaitGroup

	for i := 1; i <= 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			counter++
		}()
	}
	wg.Wait()

	fmt.Println(counter)
}
```

Kodingan di atas akan menjalankan 1000 goroutines yang dimana akan melakukan operasi increment pada variabel `counter`. Namun hasil output tidak sesuai dengan apa yang diinginkan, nilai pada variabel counter yang diinginkan adalah 1000, namun output menghasilkan 931.

```bash
Counter : 931
```

Nah disini terjadi yang namanya `Race Condition`. Race condition adalah kondisi dua atau lebih goroutine mengakses data yang sama pada waktu yang bersamaan juga. Ketika hal ini terjadi, nilai pada data tersebut akan menjadi kacau dan tidak sesuai apa yang diinginkan.

Dan masalah ini terjadi ketika ada proses yang berjalan secara asynchronous atau concurrency programming contohnya goroutines ini.

Lalu bagaimana cara mengatasinya? Ada dua cara yang bisa digunakan untuk mengatasinya, yang pertama bisa menggunakan package `sync/mutex` dan menggunakan package Atomic `sync/atomic`.

Mutex melakukan pengubahan sebuah data menjadi eksklusif, artinya hanya dapat dikonsumsi oleh satu buah goroutine saja. Cara kerjanya jika ada sebuah goroutine yang sudah mengakses suatu data, maka goroutine selanjutnya akan menunggu hingga goroutine yang sedang mengakses data tersebut selesai.

Oke langsung saja ke kodingannya:

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var counter int64
	var wg sync.WaitGroup
	var mutex sync.Mutex

	for i := 1; i <= 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			mutex.Lock()
			counter++
			mutex.Unlock()
		}()
	}
	wg.Wait()

	fmt.Println("Counter :", counter)
}
```

Nah dari contoh diatas kita melakukan yang namanya locking `mutex.Lock()` untuk proses increment dan melakukan locking pada baris 17. Dan jangan lupa juga untuk buka kembali menggunakan `mutex.Unlock()` di baris 19.

```bash
Counter : 1000
```

Dan hasil output akan sesuai apa yang diinginkan yaitu 1000.

Cara yang kedua bisa menggunakan package `sync/atomic`. Dengan kodingan seperti di bawah ini:

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	var counter int64
	var wg sync.WaitGroup

	for i := 1; i <= 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			atomic.AddInt64(&counter, 1)
		}()
	}
	wg.Wait()

	fmt.Println(counter)
}
```

Dari contoh kodingan di atas kita menggunakan package atomic di baris 17 dengan menggunakan function `addInt64(addr *int64, delta int64)`.

Di parameter pertama kita passing pointer variabel counter dan di parameter kedua adalah deltanya.

Jadi ya begitulah bagaimana cara mengatasi Race Condition pada sisi programming menggunakan bahasa pemrograman Go.

Referensi yang bisa dibaca:
- https://pkg.go.dev/sync
- https://pkg.go.dev/sync/atomic
- https://www.baeldung.com/cs/race-conditions