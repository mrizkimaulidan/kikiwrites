---
title: "Method Overloading"
date: 2022-03-23T08:58:00+08:00 # example 2020-01-01T24:00:00+08:00. Date should be the current time! you need insert it manually.
description: "Apa itu method overloading?"
tags:
- method overloading
categories:
- programming
---

Method Overloading adalah suatu kemampuan dari bahasa pemrograman yang dapat memiliki nama function atau method yang sama dalam satu class tetapi argumen atau parameternya berbeda.

Jadi ibaratnya ada dua nama function atau method yang sama tapi berisi argumen atau parameter yang berbeda disebut Method Overloading. Dari beberapa bahasa pemrograman yang memiliki Method Overloading antara lain Java dan C++.

Okelah gas ke contoh saja hehe.

Berikut adalah contoh dari bahasa pemrograman C++:

```c++
#include <iostream>

using namespace std;

void print(string name) {
  cout << "Hello my name is " << name << "." << endl;
}

void print(string name, int age) {
  cout << "Hello my name is " << name << ". " << "I am " << age << " years old.";
}

int main() {
  string name = "Rizki";
  int age = 19;  // this year yay!
  print(name);
  print(name, age);
}
```

```bash
Hello my name is Rizki.
Hello my name is Rizki. I am 19 years old.
```

Bisa dilihat dari contoh kode di atas, ada dua buah function atau method yang memiliki nama yang sama tetapi argumen atau parameternya berbeda. Pada baris nomor `5` ada sebuah function dengan nama `print` yang memiliki satu argumen atau parameter yaitu `name`.

```c++
void print(string name) {
  cout << "Hello my name is " << name << "." << endl;
}
```

Lalu di baris nomor `9` ada sebuah function lagi dengan nama yang sama yaitu `print` tapi memliki dua argumen atau parameter yaitu `name` dan `age`.

```c++
void print(string name, int age) {
  cout << "Hello my name is " << name << ". " << "I am " << age << " years old.";
}
```

Jadi kesimpulannya walaupun ada function atau method yang sama tetapi argumen atau parameternya berbeda itu tidak masalah. Tetapi untuk dari segi development bersama orang lain mungkin akan sedikit membingungkan.

Referensi:
- https://en.wikipedia.org/wiki/Function_overloading
- https://beginnersbook.com/2017/08/cpp-function-overloading/
- https://softwareengineering.stackexchange.com/questions/242610/whats-the-point-of-method-overloading