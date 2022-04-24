---
title: "Query vs Exec vs Prepare Pada Go"
date: 2022-04-24T01:55:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Apasih perbedaan Query, Exec dan Prepare pada Go"
tags:
- go
- golang
categories:
- programming
---

Akhirnya ada kesempatan untuk ngeblog, mending deh blog yang kosong daripada hati yang kosong wahaha.

Jadi ceritanya gua abis belajar tentang package `database/sql` di Go. Jadi ya iseng aja bikin catatan pribadi, siapa tau lu pada butuh juga kan.

Go ini punya built-in librarynya sendiri untuk urusan database, jadi ga perlu cape-cape untuk nyari third party untuk ngurusin database. Tapi sayang banget untuk driver database khususnya mysql masih perlu download dari third party lagi. Tapi no problem sih, semoga aja kedepannya punya package driver mysql sendiri.

## Query
Let's talk about Query, kalau mau ngelakuin proses `SELECT` alangkah baiknya selalu pake Query. Jadi ada beberapa function dengan prefix Query sebagai berikut:

- Query(query string, args ...any) (*Rows, error)
- QueryContext(ctx context.Context, query string, args ...any) (*Rows, error)
- QueryRow(query string, args ...any) *Row
- QueryRowContext(ctx context.Context, query string, args ...any) *Row

Nah masing-masing function di atas punya karakteristik dan behavior mereka masing-masing. Walaupun prosesnya sama tapi behaviornya berbeda.

**Query()** akan mengeksekusi query yang mengembalikan nilai, biasanya pada proses `READ` atau `SELECT` data. Dan khusus untuk `Query()` akan mengembalikan banyak data karena returnnya dari struct Rows.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
)

func TestQuery(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    rows, err := db.Query("SELECT id, name, balance FROM customers")
    if err != nil {
        panic(err)
    }

    for rows.Next() {
        var id, name string
        var balance int
        err = rows.Scan(&id, &name)
        if err != nil {
            panic(err)
        }

        fmt.Println("============")
        fmt.Println("ID :", id)
        fmt.Println("Name :", name)
        fmt.Println("Balance :", balance)
    }
}
```

```bash
$ go test -v -run=TestQuery
============
ID : 1
Name : Muhammad Rizki Maulidan
Balance : 2500000
============
ID : 2
Name : Ujang Batubara
Balance : 1000000
============
ID : 3
Name : Eko Putra
Balance : 5000000
```

**QueryContext()** nah kalau pengen ada context bisa pake function `QueryContext()`, siapa tau butuh kirim sinyal context kaya `context.Background()`, `context.Deadline()`, `context.WithTimeout()` etc. Dan si dia ini sama aja kaya functoin `Query()` yang pertama gua bahas, yang berbeda cuma ada contextnya doang.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
)

func TestQueryContext(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    rows, err := db.QueryContext(context.Background(), "SELECT id, name, balance FROM customers")
    if err != nil {
        panic(err)
    }

    for rows.Next() {
        var id, name string
        var balance int
        err = rows.Scan(&id, &name)
        if err != nil {
            panic(err)
        }

        fmt.Println("============")
        fmt.Println("ID :", id)
        fmt.Println("Name :", name)
        fmt.Println("Balance :", balance)
    }
}
```

```bash
$ go test -v -run=TestQueryContext
============
ID : 1
Name : Muhammad Rizki Maulidan
Balance : 2500000
============
ID : 2
Name : Ujang Batubara
Balance : 1000000
============
ID : 3
Name : Eko Putra
Balance : 5000000
```

**QueryRow()** akan menghasilkan satu baris record aja. Biasanya sih kepake untuk proses `READ` data berdasarkan keyword yang unik, contohnya kaya ambil data yang berdasarkan ID, email, nomor handphone. Ya tergantung kasus yang ditemuin aja lah. Intinya bakal mengembalikan satu baris record aja.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
)

func TestQueryRow(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    rows, err := db.QueryRow("SELECT id, name FROM customers WHERE id = ?", 1)
    if err != nil {
        panic(err)
    }

    for rows.Next() {
        var id, name string
        var balance int
        err = rows.Scan(&id, &name)
        if err != nil {
            panic(err)
        }

        fmt.Println("============")
        fmt.Println("ID :", id)
        fmt.Println("Name :", name)
        fmt.Println("Balance :", balance)
    }
}
```

```bash
$ go test -v -run=TestQueryRow
============
ID : 1
Name : Muhammad Rizki Maulidan
Balance : 2500000
```

**QueryRowContext()** nah kalau yang terakhir ini sama aja kaya yang di atas, cuma bisa mengembalikan satu baris record aja. Dan yang ngebedain dia ini pake context.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
)

func TestQueryRowContext(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    rows, err := db.QueryRowContext("SELECT id, name, balance FROM customers WHERE id = ?", 2)
    if err != nil {
        panic(err)
    }

    for rows.Next() {
        var id, name string
        var balance int
        err = rows.Scan(&id, &name)
        if err != nil {
            panic(err)
        }

        fmt.Println("============")
        fmt.Println("ID :", id)
        fmt.Println("Name :", name)
        fmt.Println("Balance :", balance)
    }
}
```

```bash
$ go test -v -run=TestQueryRowContext
============
ID : 2
Name : Ujang Batubara
Balance : 1000000
```

## Exec
Nah kalau Exec sering kepake untuk proses `CREATE`, `UPDATE` atau `DELETE` pada database. Karena fungsinya untuk mengeksekusi query, beda halnya kaya Query, kalau Query kan ada returnnya berdasarkan data apa yang mau diquery, kalau Exec engga.

Intinya Exec akan mengeksekusi tanpa mengembalikan baris record. Cuma ada dua buah function dengan prefix Exec:
- Exec(query string, args ...any) (Result, error)
- ExecContext(ctx context.Context, query string, args ...any) (Result, error)

**Exec()** yap sudah dijelasin tadi kalau Exec fungsi khusus untuk mengeksekusi query. Akan mengembalikan struct Result dan error.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
)

func TestExec(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    result, err := db.Exec("UPDATE customers SET balance = 5000000 WHERE id = ?", 2)
    if err != nil {
        panic(err)
    }

    rows, err := result.RowAffected()
    if err != nil {
        panic(err)
    }

    if rows != 1 {
        fmt.Println("Gagal merubah data", err)
    }
}
```

```bash
$ go test -v -run=TestExec
=== RUN TestExec
--- PASS: TestExec (0.00s)
```

**ExecContext()** nah si dia ini sama aja tapi bisa pake context.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
)

func TestExecContext(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    result, err := db.ExecContext(context.Background(),"DELETE FROM customers WHERE id = ?", 3)
    if err != nil {
        panic(err)
    }

    rows, err := result.RowAffected()
    if err != nil {
        panic(err)
    }

    if rows != 1 {
        fmt.Println("Gagal merubah data", err)
    }
}
```

```bash
$ go test -v -run=TestExecContext
=== RUN TestExecContext
--- PASS: TestExecContext (0.00s)
```

## Prepare
Prepare bagusnya kepake kalau misalkan pengen mengeksekusi banyak query tapi dengan query yang sama.

Sebagai contoh, kalau Query dan Exec itu dibelakang layar akan membuat banyak koneksi pooling ke database karena perintahnya dilakukan berulang-ulang tapi dengan koneksi database pooling yang berbeda juga.

Nah sedangkan Prepare `hanya` akan `sekali` menggunakan koneksi database pooling dan akan disimpan jika dibutuhkan lagi dan `hanya` menggunakan `satu koneksi database pooling`.

So? dalam segi memory dan performance lebih bagus Prepare. Bayangin aja dari client mengeksekusi banyak koneksi, contoh deh ratusan atau ribuan ke database, ya kalau ratusan dan ribuan masih kecil sih tapi kan mahal banget! Di lain sisi juga ngeberatin server dan database.

Atau contoh lain misalnya mengeksekusi `INSERT` data berdasarkan dari file excel. Kan ga ada bedanya, paling yang beda nanti pas di bagian ID nya dan datanya tapi querynya sama. Ga kebayang ada ribuan baris excel dieksekusi satu-persatu dan berulang-ulang tanpa Prepare statement, bakal down deh tuh server haha.

Ada dua buah function dengan prefix Prepare sebagai berikut:
- Prepare(query string) (*Stmt, error)
- PrepareContext(ctx context.Context, query string) (*Stmt, error)

**Prepare()** jadi Prepare akan mengeksekusi sebuah prepare statement, dan statement tersebut akan disimpan jika dibutuhkan lagi dan reusable.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
    "strconv"
    "testing"
)

func TestPrepare(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    statement, err := db.Prepare("INSERT INTO customers(id, name, balance) VALUES(?, ?, ?)")
    if err != nil {
        panic(err)
    }
    defer statement.Close()

    for i := 1; i <= 10; i++ {
        name := "Nama Seseorang ke-" + strconv.Itoa(i)
        balance := 100000 + i

        result, err := statement.Exec(name, balance)
        if err != nil {
            panic(err)
        }
    }
}
```

```bash
$ go test -v -run=TestPrepare
=== RUN TestPrepare
--- PASS: TestPrepare (0.00s)
```

Nah dari kodingan diatas akan melakukan 10 kali eksekusi `INSERT` ke database dengan menggunakan prepare statement. Jadi proses dibelakang layar hanya menggunakan satu koneksi database pooling aja.

**PrepareContext()** nah kalau yang ini sama aja tapi yang bedanya dia ini menggunakan context.

```go
package main

import (
    _ "github.com/go-sql-driver/mysql"
    "fmt"
    "database/sql"
    "strconv"
    "testing"
)

func TestPrepareContext(t *testing.T) {
    db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/olshop")
    if err != nil {
        panic(err)
    }
    defer db.Close()

    statement, err := db.PrepareContext(context.Background(), "INSERT INTO customers(id, name, balance) VALUES(?, ?, ?)")
    if err != nil {
        panic(err)
    }
    defer statement.Close()

    for i := 1; i <= 10; i++ {
        name := "Nama Seseorang ke-" + strconv.Itoa(i)
        balance := 100000 + i

        result, err := statement.ExecContext(name, balance)
        if err != nil {
            panic(err)
        }
    }
}
```

```bash
$ go test -v -run=TestPrepareContext
=== RUN TestPrepareContext
--- PASS: TestPrepareContext (0.00s)
```

Referensi:
- https://pkg.go.dev/database/sql
- https://zetcode.com/golang/mysql/
- https://tutorialedge.net/golang/golang-mysql-tutorial/