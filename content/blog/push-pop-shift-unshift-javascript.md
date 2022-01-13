---
title: "Push, Pop, Shift & Unshift Javascript"
date: 2021-11-19T13:17:00+08:00
description: "Membahas push, pop, shift dan unshift pada bahasa pemrograman Javascript"
tags:
- javascript
categories:
- programming
---

Javascript menawarkan built-in function untuk menambah atau menghapus elemen pertama atau yang terakhir di dalam array.

##### Push
Method `push()` method yang menambahkan satu atau lebih dari satu elemen string ke array paling terakhir dan mengembalikan hasil keseluruhan elemen arraynya.

```javascript
let persons = ['Buday', 'Joko']
// Array : ['Buday', 'Joko']

persons.push('Kiki')

console.log(persons)
// Array : ['Buday', 'Joko', 'Kiki']

```

##### Pop
Method `pop()` menghapus elemen array paling terakhir dari array dan mengembalikan hasil keseluruhan elemen arraynya.

```javascript
let plants = ['Tomato', 'Cabbage', 'Potato']
// Array : ['Tomato', 'Cabbage', 'Potato']

plants.pop()

console.log(plants)
// Array : ['Tomato', 'Cabbage']
```

##### Shift
Method `shift()` menghapus elemen pertama dari array dan mengembalikan keseluruhan arraynya.

```javascript
let friends = ['Jack', 'Bob', 'Rick']
// Array : ['Jack', 'Bob', 'Rick']

let removed = friends.shift()

console.log(removed)
// String : Jack

console.log(friends)
// Array : ['Bob', 'Rick']
```

##### Unshift
Method `unshift()` method yang menambahkan satu atau lebih dari satu elemen string ke array paling depan dan mengembalikan hasil keseluruhan elemen arraynya.

```javascript
let federalAgencies = ['FBI', 'CIA', 'NASA']

federalAgencies.unshift('KGB')

console.log(federalAgencies)
// Array : ['KGB', 'FBI', 'CIA', 'NASA']
```