---
title: JavaScript 觀念 - var、let、const 差異
date: 2022-09-26
tags:
 - Scope
categories: JavaScript
---


JavaScript 變數的宣告方式有三種，分別是 `var`、`let` 以及 `const`，後兩者是 ES6 新增的語法，而這三種方式所宣告的變數也會有不同的作用。

## 重複宣告

最基本的差別就是變數是否能夠被重複宣告，以 `var` 來說，重複宣告的行為並不會出現錯誤：

```js
var a = 1;
var a = 2;
```

而 `let` 如果重複宣告，就會出現變數已經被宣告的錯誤訊息，如下：

```js
let a = 1;
let a = 2;
// Uncaught SyntaxError: Identifier 'a' has already been declared
```

不過 `let` 雖然無法重複宣告，但是能夠重新賦值，如下：

```js
let a = 1;
a = 2;
console.log(a);
// 2
```

最後 `const` 則是不可重複宣告，也無法重新賦值，如下：

```js
const a = 1;
const a = 2;
// Uncaught SyntaxError: Identifier 'a' has already been declared
```

```js
const a = 1;
a = 2;
// Uncaught TypeError: Assignment to constant variable.
```



## 作用域

「作用域（Scope）」可以理解成一個變數能夠被存取的範圍。

