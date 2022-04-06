---
title: "【JavaScript】陣列中的 map()、filter()、reduce() 方法"
date: 2022-03-22
categories:
  - blog
tags:
  - javaScropt
  - Array
---

陣列 Array 介紹 / 陣列中的 map()、filter()、reduce() 方法

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*eVCEJzH6xLSof2ql.jpeg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">photo by Gabriel Heinzer on Unsplash</div>
</center>

> 前提：初學者的學習筆記，僅供參考，敬請指教～

## 陣列 Array 介紹

陣列的操作在 JavaScript 中相當重要，但在討論陣列的方法之前，讓我們先來認識什麼是陣列（Array），詳細內容可以參考 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#examples) 對陣列的說明與描述，這裡引用說明的部分：

> **Array**
> The Array object, as with arrays in other programming languages, enables storing a collection of multiple items under a single variable name, and has members for performing common array operations.

簡單來說，JS 中的陣列能夠使用變數定義、存放複數的資料型態，且可以使用一般的陣列方法操作。MDN 的說明中將陣列稱為物件，因為在 JS 的世界中，除了基本資料型態（字串、數字、布林值）以外都是物件，如果我們使用 `typeof []` 檢查型態，會返回 `object`，所以我們其實可以把陣列看成一種特殊的的物件，但一般來說我們還是會把它叫做陣列。【註 1】

在[W3C](https://www.w3schools.com/js/js_arrays.asp) 就有特別指出陣列跟物件的不同處：

> **The Difference Between Arrays and Objects**
>
> In JavaScript, arrays use numbered indexes.  
> In JavaScript, objects use named indexes.
>
> Arrays are a special kind of objects, with numbered indexes.

陣列是個有順序性的集合體，使用數字進行索引，而物件則是使用名字（key）進行索引。再更白話的說明，陣列其實是一個儲存資料的盒子，而且是有順序性的，當我們要拿裡面的資料，去抓它的位置就行。

另外提醒，雖然陣列並沒有規定它能或不能放什麼資料進去，但通常不建議放入不同的資料型態，會造成操作上的困難。比如下面一格一格的藥盒，雖然可以不放藥片進去，但塞其他東西進去就是怪怪的吧。

![](https://i.imgur.com/im6nRwW.jpg)

---

JS 中有許多針對陣列操作的方法，適用的情境各不相同。今天就來介紹 `map()`、`filter()`、`reduce()`。但在在學習陣列的操作方法之前，我們需要注意：

1. 方法的回傳值，就是這個方法到底丟給我們什麼東東。
2. 原本的陣列怎麼了，這個方法會不會改變原本的陣列。

要注意以上兩點，才能正確使用這些方法。

## Array.prototype.map()

`map()` 方法會建立一個新的陣列，其內容為原陣列的每一個元素經由回呼函式運算後所回傳的結果之集合。語法如下：

```javascript
let new_array = arr.map(function callback( currentValue[, index[, array]]) {
    // return element for new_array
}[, thisArg])
```

- currentValue： 代表陣列資料的內容。
- index： 索引位置，從 0 開始。
- array： 代表陣列資料本身。

白話文：**對每個元素做一些事，然後收集成⼀個新的陣列。**

使用 `map()` 們得到的回傳值為一個經過運算的新陣列，且不會改變原來的陣列。

- 範例如下：

```javascript
// map 對每個元素做些事，然後收集成⼀個新的陣列
let heroes = ["孫悟空", "魯夫", "宇智波佐助", "琦⽟"];
const superHeroes = heroes.map(function (hero) {
  return "超級" + hero;
});
console.log(superHeroes);
// [ '超級孫悟空', '超級魯夫', '超級宇智波佐助', '超級琦⽟' ]
// heroes 和 superHeroes 是兩個不同的陣列
```

另外，`map()` 的遍歷和 `forEach` 很像，並且和 for 迴圈的功能類似，讓我們來看看同一個題目如何用這三種方法去達成：

- 【題目】將陣列中元素的值變成兩倍，得到一個新的陣列。使用`for迴圈` `map()` `forEach()`：

```javascript
// for 迴圈
const list = [1, 2, 3, 4, 5];
let result = [];
for (i = 0; i < list.length - 1; i++) {
  result[i] = list[i] * 2;
}

console.log(result); //  [2, 4, 6, 8, 10]
```

```javascript
// forEach
const list = [1, 2, 3, 4, 5];
let newList = [];
list.forEach(function (x) {
  newList.push(2 * x); //push
});

console.log(newList); //  [2, 4, 6, 8, 10]
```

```javascript
// map()
const result = list.map(function (x) {
  return x * 2;
});

console.log(result); //  [2, 4, 6, 8, 10]
```

## Array.prototype.filter()

`filter()` 方法會建立一個經指定之函式運算後，由原陣列中通過該函式檢驗之元素所構成的新陣列。語法如下：

```javascript
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
```

- element： 代表陣列資料的內容。
- index： 索引位置，從 0 開始。
- array： 代表陣列資料本身。

白話文：**做條件判斷，只要條件為 true ，就會蒐集成一個新的陣列。**

使用 `filter()`，若為 true，我們會得到一個回傳值為一個經篩選的新陣列；若為 false ，則得到一個空陣列。都不會改變原來的陣列。

```javascript
let heroes = ["孫悟空", "魯夫", "宇智波佐助", "琦⽟"];
// filter 對每個元素做些判斷
// 把符合條件的元素收集成⼀個新的陣列
const newHeroes = heroes.filter(function (hero) {
  return hero.length == 2; // 名字只有 2 個字的英雄
});
console.log(newHeroes); // [ '魯夫', '琦⽟' ]
```

由於只要條件為 true 的就會包含在此陣列，`filter()` 適合拿來搜尋。

---

## Array.prototype.reduce()

`reduce()`方法將一個累加器及陣列中每項元素（由左至右）傳入回呼函式，將陣列化為單一值。語法如下：

```javascript
arr.reduce(
  callback[(accumulator, currentValue, currentIndex, array)],
  initialValue
);
```

- accumulator：代表上一個參數。
- currentValue：代表當前的資料。
- currentIndex：代表索引。
- array：代表陣列資料本身。
- initialValue：代表初始值，若沒有提供初始值，則原陣列的第一個元素將會被當作初始值。

白話文：**與前一個回傳值做運算，並且回傳運算結果的值。**

使用 `reduce()`，我們可以得到一個回傳值為「經過運算的值」，原本的陣列不會改變，`reduce()` 常應用在累加的方法上。

- 範例如下：

```javascript
let scores = [87, 65, 92, 75, 98, 100];
// reduce 每回合會傳進「累加值」跟「⽬前這個元素」
// ⽽「回傳值」就會變成下⼀回的「累加值」
// Accumulator 累加值；current 目前
const totalScore = scores.reduce(function (acc, current) {
  return acc + current;
});
console.log(totalScore); // 印出總和 517
```

```javascript
const list = [1, 2, 3, 4, 5];

let a = list.reduce(function (acc, cv) {
  return acc + cv;
}, 100);
// acc沒有設定預設值，則從陣列第一個元素開始
// 沒有初始值，迴圈會少跑一圈
console.log(a); // 15
```

```javascript
// reduce 找出最大值
const list = [19, 23, 3, 2, 24];
let a = list.reduce(function (acc, cv) {
  if (cv > acc) {
    return cv;
  }
  return acc;
});

console.log(a); //24
```

```javascript
const list = [19, 23, 3, 2, 24, 8];
let a = list.reduce(function (acc, cv) {
  console.log("acc=", acc, "cv=", cv);
  if (cv > acc) {
    return cv;
  }
  return 1;
});

console.log(a);　
acc= 19 cv= 23
// acc= 23 cv= 3
// acc= 1 cv= 2
// acc= 2 cv= 24
// acc= 24 cv= 8
// 1
```

---

【註 1】：比較精確的類型判斷可使用 `instanceof` 。用法為：`A instanceof B`，如果 A 是 B 的類型，則返回 true，否則返回 false。例如：

```javascript
console.log([] instanceof Array); // true
```
