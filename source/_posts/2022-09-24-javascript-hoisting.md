---
title: JavaScript 觀念 - 提升
date: 2022-09-24
tags:
 - hoisting
categories: JavaScript
index_img: img/banner/banner_js.jpg
---

ES6 前後的 Hoisting 觀念筆記。

<!--more-->


「提升（Hoisting）」是 JavaScript 特有的一種現象，它的意思是當一個變數或函式在被宣告之前就可以被使用，並且不會出現錯誤，另外 ES6 以前都是使用 `var` 來宣告變數，與 ES6 新增的 `let`、`const` 在提升行為會也有所不同。

## ES6 以前的提升

```js
console.log(x);
// ReferenceError: x is not defined
```

以上針對 `x` 取值，但是在這之前並沒有宣告一個名為 `x` 的變數，因此會找不到該變數而回報錯誤，下面是正常的做法：

```js
var x;
console.log(x);
// undefined
```

這樣就是一個正常的流程，程式碼會從第一行開始由上往下執行，所以在使用變數之前需要先宣告變數，以確保這個變數是存在的。

接著下面嘗試把第 1、2 行位置進行對調：

```js
console.log(x);
var x;
// undefined
```

按照前面的說法，上述程式碼應該會回報錯誤，但是得到的結果卻是 `undefined`，而這就是提升所造成的現象，感覺像是 `var x` 這段程式碼被提升到所有程式碼之前。

大多數的程式語言中，變數在被使用之前是需要先宣告的，但是 JavaScript 可以在變數宣告之前就使用該變數，不過關於提升還有一些注意事項，就是會被提升的只有**宣告的行為**，值的指派並不會被提升，以下面程式碼為例：

```js
var x = 7; // 宣告變數並指派值
console.log(x);
// 7
```

範例跟前面的相同，只是多了一個指派數值的動作，而最後也印出預期的結果，這個時候如果再將 1、2 行位置進行對調，印出的結果會是 `7` 嗎？

```js
console.log(x);
var x = 7;
// undefined
```

答案是 `undefined`，照理來說宣告的變數會被提升，可是得到的結果卻不是 `7`，原因其實就如前面提到的，會被提升的只有宣告的動作，而範例中宣告的動作指的是 `var x`，後方的 `= 7` 屬於值的指派，並不會跟著宣告一起被提升。

但是結果為甚麼會是 `undefined`？原因在於 JavaScript 在開始執行你撰寫的程式碼之前，會先把所有宣告的變數、一般函式都預留一個記憶體空間，但不會馬上指派值給變數，這個預留記憶體空間的動作就是提升，到這邊為止屬於「創造階段」，而這個階段結束之後，變數才會被賦值，這個賦值的過程則是「執行階段」，`undefined` 就是變數在創造階段建立記憶體空間時，預設給定的初始值。

可以將上面的程式碼運作流程理解成以下形式：

```js
var x; // 宣告變數（會被提升）
console.log(x);
x = 7; // 指派值
```

目前為止已經知道會被提升的只有宣告的動作，但是除了變數之外，一般的函式宣告也會被提升，以下面程式碼為例：

```js
fn();
function fn() {
  console.log('hello');
}
// hello
```

嘗試在函式被宣告之前呼叫，也可以順利執行且不會回報錯誤，因為這是一般的函式宣告（具名函式），整個函式都會被提升，因此就可以在函式宣告之前呼叫。

那匿名函式就不會被提升嗎？答案是會，下面嘗試將範例改成以匿名函式的方式建立：

```js
fn();
var fn = function() {
  console.log('hello');
}
// TypeError: fn is not a function
```

結果出現錯誤了，不過是不是與 `var` 宣告的變數很像？其實概念是一樣的，這裡的程式碼確實有被提升，但是被提升的只有變數的宣告 `var fn`，函式的資料在執行階段才會指派給變數 `fn`，此時的 `fn` 的值為 `undefined`，而 `undefined` 並非函式因此呼叫的行為就會回報錯誤。

雖然無法呼叫，但是能透過變數取值來驗證上述說法：

```js
console.log(fn);
var fn = function() {
  console.log('hello');
}
// undefined
```

而程式碼實際運作流程就像下面這樣：

```js
var fn;
console.log(fn);
fn = function() {
  console.log('hello');
}
```

總結來說，JavaScript 中的「提升（Hoisting）」指的是宣告變數的提升，而值的賦予並不會提升；此外，提升並不會變更程式碼的位置，只是感覺像是整段程式碼被移動到最上方。



## let、const 的提升

關於 `let`、`const` 有沒有提升行為，一開始我自己也是透過以下方法來作結論的：

```js
console.log(a); // ReferenceError: a is not defined
let a = 1;
```

原本看到上面的結果，是認為 `let`、`const` 沒有提升行為的，直到看到一篇文章寫了下面這段程式碼：

```js
var a = 1;
function fnA() {
  console.log(a);
  let a = 100;
}
fnA();
// ReferenceError: Cannot access 'a' before initialization
```

可以看到在函式 `fnA` 裡面，嘗試在變數 `a` 被宣告之前取值，照理來說，如果 `let` 沒有提升行為，第 3 行的變數 `a` 應該會指向到函式外層的變數 `a`，因此印出結果應該是 `1` 才對，但是最終卻得到 `Cannot access 'a' before initialization` 的錯誤訊息，而這就證明了 `let` 是有提升行為的，只是不允許在變數宣告之前被存取。

而前面提到 `var` 在提升時，也就是創造階段預設會給定初始值 `undefined`，而 `let`、`const` 則不會，所以在變數實際賦值前嘗試存取就會出現上面的錯誤，而提升後到賦值之前的這一個區間，稱為「暫時性死區（TDZ）」。





## 參考資料

[我知道你懂 hoisting，可是你了解到多深？](https://blog.techbridge.cc/2018/11/10/javascript-hoisting/)