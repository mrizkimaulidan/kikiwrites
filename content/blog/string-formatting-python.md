---
title: "String Formatting Python"
date: 2021-11-11T22:20:00+08:00
description: "Jenis-jenis string formatting pada bahasa pemrograman Python"
tags:
- python
categories:
- programming
---

Ada 4 cara utama untuk menggunakan string formatting dengan Python.

1. Printf Style Formatting ([Docs](https://docs.python.org/3/library/stdtypes.html#old-string-formatting))
2. String Formatting (str.format) ([Docs](https://docs.python.org/3/library/stdtypes.html#str.format))
3. String Interpolation / f-Strings (Python 3.6+) ([Docs](https://www.python.org/dev/peps/pep-0498/))

## 1. Printf Style Formatting
String dalam Python memiliki operasi bawaan unik yang dapat diakses dengan operator %. Ini memungkinkan melakukan pemformatan posisi sederhana dengan sangat mudah. Jika pernah menggunakan fungsi printf di C, mungkin akan langsung mengenalinya.

```python
name = 'Rizki'

print('Halo %s!' % name)
# Halo Rizki!
```

Saya menggunakan penentu format %s di sini untuk memberi tahu Python tempat untuk mengganti nilai nama, yang direpresentasikan sebagai string.

```python
name = 'Rizki'
job = 'Mahasiswa'

print('Perkenalkan nama saya %s, dan saya adalah seorang %s!' % (name, job))
# Perkenalkan nama saya Rizki, dan saya adalah seorang Mahasiswa!
```

Ini membuat string format lebih mudah dimaintain dan lebih mudah dimodifikasi di masa mendatang. Tidak perlu khawatir untuk memastikan urutan nilai yang diberikan cocok dengan urutan nilai yang dirujuk dalam format string. Kelemahannya adalah teknik ini membutuhkan lebih banyak pengetikan.

## 2. String Formatting (str.format)
Python 3 memperkenalkan cara baru untuk melakukan pemformatan string yang juga kemudian di-porting kembali ke Python 2.7. Pemformatan string ini menghilangkan sintaks khusus %-operator dan membuat sintaks untuk pemformatan string lebih teratur. Pemformatan sekarang ditangani dengan memanggil .format() pada objek string. Contohnya seperti kode di bawah ini :

```python
name = 'Rizki'

print('Halo {}!'.format(name))
# Halo Rizki!
```

Atau bisa merujuk ke substitusi variabel dengan nama. Ini adalah fitur yang cukup bagus karena memungkinkan untuk mengatur ulang tampilan tanpa mengubah argumen yang diteruskan ke format() :

```python
weather = 'Dingin'
food = 'Mie Kuah'

print('Cuaca hari ini {weather}. Sepertinya enak makan {food}!'.format(weather=weather, food=food))
# Cuaca hari ini Dingin. Sepertinya enak makan Mie Kuah!
```

## 3. String Interpolation / f-Strings (Python 3.6+)
Python 3.6 menambahkan string formatting yang disebut literals atau `f-Strings`. Menggunakan ekspresi Python yang disematkan di dalam sebuah string. Contohnya seperti kode di bawah ini :

```python
name = 'Rizki'

print('Halo nama saya {name}!')
# Halo nama saya Rizki!
```

Seperti yang bisa dilihat di atas, untuk formatting ini diawali dengan huruf `f` oleh karena itu disebut `f-Strings`. Sintaks pemformatan ini sangat powerful. Atau bahkan dapat melakukan proses aritmatika seperti kode di bawah ini :

```python
a = 5
b = 10

print(f'Lima ditambah dengan Sepuluh sama dengan {a + b}.')
# Lima ditambah dengan Sepuluh sama dengan 15.
```

```python
def sayHello(name, question):
    return f'Halo {name}!. Bagaimana {question}?'

result = sayHello('Rizki', 'kabarnya hari ini')
# Halo Rizki!. Bagaimana kabarnya hari ini?
```

Dari 4 string formatting di atas, saya lebih sering menggunakan String Interpolation / f-Strings untuk pemformatan string ketika menggunakan Python. Tapi tergantung kasus, tidak selamanya juga saya menggunakan cara nomor 3.