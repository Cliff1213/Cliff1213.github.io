---
title: JavaScript 觀念 - var、let、const 有什麼不同
date: 2022-09-21
tags:
 - Scope
categories: JavaScript
index_img: img/banner/banner_js.jpg
---

同樣都是用來宣告變數，為什麼還要分成三種方式？到底 let、const 解決了什麼問題？

<!--more-->


JavaScript 變數的宣告方式有三種，分別是 `var`、`let` 以及 `const`，後兩者是 ES6 新增的語法，而這三種方式所宣告的變數也會有不同的作用。

## 重複宣告

透過 `var` 宣告的變數，重複宣告的行為不會回報錯誤：

```js
var a = 1;
var a = 2;
```

雖然 JavaScript 允許上面這種行為，但是一般情況下，一個變數只會進行一次宣告。

`let` 不可重複宣告，但是可以重新賦值：

```js
let a = 1;
let a = 2;
// SyntaxError: Identifier 'a' has already been declared
```

```js
let a = 1;
a = 2;
console.log(a); // 2
```

`const` 則是不可重複宣告，也無法重新賦值：

```js
const a = 1;
const a = 2;
// SyntaxError: Identifier 'a' has already been declared
```

```js
const a = 1;
a = 2;
// TypeError: Assignment to constant variable.
```


## 作用域

「作用域（Scope）」可以理解成一個變數能夠被存取/使用的範圍。ES6 之前的作用域有兩種，分別是**全域作用域**以及**函式作用域**，而 ES6 新增了**區塊作用域**。

### 全域作用域（Global Scope）

不是在 `function` 或 `block` 裡面宣告的變數，作用域就是全域。

```js
var a = 1;
function fn() {
  console.log(a + 1);
}
for(let i=0; i<=5; i++) {
  a += i;
  console.log(a);
}
```

上方範例中的變數 `a` 是在全域環境下被宣告的，而全域變數可以在檔案中的任何地方存取。

### 函式作用域（Function Scope）

```js
var a = 1;
function fn() {
  var b = 0;
}
fn();
console.log(a, 'global'); // 1 "global"
console.log(b, 'function'); // ReferenceError: b is not defined
```

上方範例嘗試在全域環境下取得變數 `b` 值，結果會因為沒有這個變數而出現錯誤，原因在於 `var` 宣告的變數是屬於**函式作用域**，而變數 `b` 是在函式 `fn` 裡面被宣告的，因此就只能在這個函式裡面被使用，無法從函式外存取該變數。

而函式裡面可以使用函式外面的變數，如果在函式裡找不到變數，預設就會向外查找是否有相同名稱的變數。

```js
var a = 1;
var b = 2;
function fn() {
  var b = 20;
  console.log(a + b);
}
fn();
// 21
```

上方範例第 5 行因為函式內不存在 `a` 變數，因此就會使用外層的全域變數 `a`，而 `b` 因為函式內已宣告，所以即使全域環境下也有同名的變數 `b`，也會優先使用函式裡面的變數，而全域變數 `b` 與函式中的區域變數 `b` 彼此是不同的兩個變數。

同樣都是函式作用域，那兩個不同函式的變數可以互相存取嗎？

```js
function fnA() {
  var a = 'hello';
}
function fnB() {
  var b = 'world';
  console.log(a + b);
}
fnA();
fnB();
// ReferenceError: a is not defined
```

答案是不行，函式裡面宣告的變數，存取範圍只能在當前的函式或是內層的函式。

### 區塊作用域（Block Scope）

ES6 之前只能只能使用 `function` 來定義作用域，ES6 開始因為新增了 `let`、`const` 兩種宣告方式，使變數能夠以區塊（Block）來規範作用域，而區塊作用域指的是一對大括號 `{}` 裡面的範圍，常見區塊像是 `if` 判斷式、`for` 迴圈等等。

```js
{
  var a = 1;
  let b = 0;
  const c = true;
}
console.log(a, b, c);
// 1
// ReferenceError: b is not defined
// ReferenceError: c is not defined
```

以上可以看到區塊外無法存取區塊內透過 `let`、`const` 宣告的變數，因為 `var` 宣告的變數是函式作用域，所以變數 `a` 並不會被區塊限制作用域，因此作用域會是全域。

以 `if` 判斷式為例：

```js
if(true) {
  let a = 1;
}
console.log(a);
// 1
// ReferenceError: a is not defined
```

變數 `a` 因為是在 `if` 區塊中宣告的，因此無法從區塊之外存取該變數。

到這邊可以知道一件事，無論變數的作用域是函式還是區塊，都能夠存取外層的變數，外層則無法存取內層變數，為單向性。

### 變數與物件屬性

使用 `var`、`let`、 `const` 與不做關鍵字宣告行為有甚麼差別？

```js
var a = 1;
let aa = 10;
aaa = 100;
```

透過 `var` 宣告變數後，會在 window（全域物件）下新增一個同名**屬性**，`let`、`const` 則不會。

![](https://i.imgur.com/NWJJi9r.png)

從圖中 window 物件下也可以看到 `var` 宣告的變數 `a`，但是 `let` 宣告的變數 `aa` 就沒有出現在 window 物件下，另外 window 屬於全域物件，因此也可以下方式來取得物件中屬性：

```js
console.log(window.a); // 1
console.log(window.aa); // undefined
```

若不使用關鍵字來宣告，結果與使用 `var` 一樣都會在 window 物件下新增一個同名屬性，而差別就在於是否能被刪除，以下嘗試使用 `delete` 刪除變數 `a`：

```js
var a = 1;
delete window.a;
console.log(window.a); // 1
```

以上面結果來看，變數 `a` 並沒有被刪除；接著使用 `delete` 刪除屬性 `aaa`：

```js
aaa = 100;
delete window.aaa;
console.log(window.aaa); // undefined
```

可以發現 `aaa` 成功被刪除了。

因此，使用關鍵字 `var` 宣告的變數無法被刪除，而沒有宣告行為的全域屬性，是可以被刪除的。


最後再補充一下 `var`、`let` 與 `const` 的使用時機：

- 預設都以 `const` 為主
- 當變數需要重新指向時使用 `let`
- 盡可能避免使用 `var`