---
title: "Graceful Shutdown"
date: 2022-07-03T14:57:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Let's talk about graceful shutdown"
tags:
- go
- golang
- software development
categories:
- programming
---

Graceful shutdown adalah mekanisme ketika server dimatikan, tapi tidak akan langsung dimatikan, tetapi akan menunggu dulu ketika ada sebuah request yang sedang diproses diselesaikan terlebih dahulu.

Pernah ga kepikiran kalau semisalkan kita mematikan server, request yang sedang diproses apakah akan lanjut diproses atau langsung terhenti? Nah kalau kita ga pakai graceful shutdown, request tersebut akan terhenti tanpa dilanjutkan prosesnya.

Kalau menggunakan graceful shutdown, ketika server dimatikan, dia nunggu dulu nih request yang ada diproses dulu, nah setelah itu server dimatikan.

Bingung ga? bingung lah haha, ntar deh kalau udah masuk ke praticalnya bakal ngerti.

Di contoh ini gua akan menggunakan bahasa pemrograman Go, disini gua akan membuat sebuah endpoint API sederhana.

Let's code!

Yang pertama kita buat handler terlebih dahulu

```go
func index(w http.ResponseWriter, r *http.Request) {
	json.NewEncoder(w).Encode(map[string]any{
		"code":    http.StatusOK,
		"message": "server up",
	})
}
```

Dan buat main function

```go
func main() {
	srv := &http.Server{
		Addr:    "localhost:3000",
		Handler: nil,
	}

	http.HandleFunc("/", index)

	log.Println("server running at", srv.Addr)
	if err := srv.ListenAndServe(); err != nil {
		log.Fatal("error listening", err)
	}
}
```

Ya sederhana aja, bakal return JSON.

```bash
PS C:\Users\ASUS> curl http://localhost:3000

StatusCode        : 200
StatusDescription : OK
Content           : {"code":200,"message":"server up"}

RawContent        : HTTP/1.1 200 OK
                    Content-Length: 35
                    Content-Type: text/plain; charset=utf-8
                    Date: Sun, 03 Jul 2022 07:24:21 GMT

                    {"code":200,"message":"server up"}

Forms             : {}
Headers           : {[Content-Length, 35], [Content-Type, text/plain; charset=utf-8], [Date, Sun, 03 Jul 2022 07:24:21
                    GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 35
```

## Skenario tanpa menggunakan graceful shutdown

Nah sekarang kita buat skenario tanpa menggunakan graceful shutdown. Normalnya ketika kita mematikan server tanpa graceful shutdown, maka server akan langsung dimatikan, ga peduli deh ada suatu request yang sedang diproses atau engga pokoknya langsung dimatiin.

Yang pertama kita akan membuat skenario delay ketika mengakses endpoint APInya, kita pakai `time.Sleep()` aja.

```go
func index(w http.ResponseWriter, r *http.Request) {
	time.Sleep(10 * time.Second) // skenario delay 10 detik mengakses endpoint root /
	json.NewEncoder(w).Encode(map[string]any{
		"code":    http.StatusOK,
		"message": "server up",
	})
}
```

![Image 1](blog/graceful-shutdown/1656834304.gif)

Dari GIF di atas gua mengirim request ke endpoint root dengan delay 10 detik. Yes nothing happen dan proses curl berhasil diproses, but what will happen if we turning off the server ketika request curl sedang diproses? (jaksel banget dah ah)

![Image 2](blog/graceful-shutdown/1656834759.gif)

GIF di atas adalah skenario ketika gua mematikan server tapi ada request curl yang sedang diproses. Oke servernya mati, tapi request curl yang sedang diproses akan langsung terhenti juga.

Well that's not good tho, sebaiknya semisalkan servernya dimatikan, kenapa ga selesaikan dulu proses yang ada? setelah itu matikan servernya.

Nah kita akan bahas masalah itu di chapter di bawah ini.

## Skenario menggunakan graceful shutdown

Nah sekarang skenario menggunakan graceful shutdown, bakal banyak nih kodingan yang berubah disini, semoga lu paham aja ngebacanya ya haha.

Step:
1. Running server menggunakan goroutine.
2. Buat channel untuk menerima sinyal SIGTERM atau Interrupt.
3. Shutdown server menggunakan context timeout 30 detik.

Kenapa menggunakan channel? karena channel sifatnya blocking, nantinya ketika receive sinyal interrupt bakal dimasukkan ke dalam tipe data channel, ketika channelnya terisi maka ga akan blocking lagi dan akan lanjut ke proses shutdown.

Mostly kodingan yang bakal banyak berubah banyak di main function

```go
func main() {
	srv := &http.Server{
		Addr:    "localhost:3000",
		Handler: nil,
	}

	http.HandleFunc("/", index)

	// run server menggunakan goroutine
	go func() {
		log.Println("server running at", srv.Addr)
		if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
			log.Fatal("error listening", err)
		}
	}()

	// buat channel dan terima sinyal interrupt atau sigterm
	c := make(chan os.Signal, 1)
	signal.Notify(c, os.Interrupt)
	signal.Notify(c, syscall.SIGTERM)

	// masukkin data channel ke variabel sig
	// dan close channelnya
	sig := <-c
	log.Println("got signal", sig)
	defer close(c)

	// buat context dengan timeout
	// 30 detik
	ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
	defer cancel()

	// proses shutting down server
	log.Println("shutting down server..")
	if err := srv.Shutdown(ctx); err != nil {
		log.Fatal("error shutting down server", err)
	}

	log.Println("server shutted down successfully")
}
```

Semoga lu paham ya sama kodingan di atas, udah gua kasih komentar juga hehe. Sebenarnya itu implementasi dari flowchart step yang sudah gua bahas di atas.

![Image 3](blog/graceful-shutdown/1656837187.gif)

Nah GIF di atas adalah skenario ketika menggunakan graceful shutdown.

Perhatiin deh gua ngelakuin request curl ke endpoint APInya, tapi tiba-tiba servernya dimatikan, nah servernya ga bakal langsung mati, tapi nunggu dulu proses curlnya diselesaikan dulu, ketika udah selesai.. setelah itu servernya mati.

Coba lu sandingkan sama GIF kedua yang di chapter skenario tanpa graceful shutdown, keliatan deh perbedaannya gimana.

Gimana? keren kan haha.

So yeah itu point utama tentang graceful shutdown. Sebenarnya ya lebih ke implementasi di backend cara ini. Graceful shutdown ga hanya bisa diimplementasikan di bahasa pemrograman Go, bisa di bahasa pemrograman apa aja kok, contohnya kaya Node JS atau Java Spring Boot.

Referensi:
- https://gist.github.com/mrizkimaulidan/7e5886db288a41e362de53d7a7d2461a (full kodingan di atas)
- https://mariocarrion.com/2021/05/21/golang-microservices-graceful-shutdown.html