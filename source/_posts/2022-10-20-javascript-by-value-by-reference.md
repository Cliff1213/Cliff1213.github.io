---
title: JavaScript 觀念 - 傳值、傳參考
date: 2022-10-20 01:20:53
tags:
 - call by reference
categories: JavaScript
index_img: img/banner/banner_js.jpg
---

什麼情況下是傳遞純值，什麼情況又是傳遞記憶體參考位址？

<!--more-->

---
<div class="toc">
<p class="toc-title">目錄：</p>

- [JavaScript 型別](#JavaScript-型別)
- [原始型別的「傳值」（Call By Value）](#原始型別的「傳值」（Call-By-Value）)
- [物件型別的「傳參考」（Call By Reference）](物件型別的「傳參考」（Call-By-Reference）)
- [例外情況 / Call By Sharing](#例外情況-/-Call-By-Sharing)
- [小結](#小結)
- [參考資料](#參考資料)
</div>

## JavaScript 型別
JS 支援的型別主要分為以下兩種：
- 原始型別 / 基本型別（Primitives）：`string`、`number`、`boolean`、`null`、`undefined`、`symbol`（ES6 新增），原始型別也表示這個資料是一個「純值」。
- 物件型別（Object）：非基本型別的類型都屬於物件型別（陣列、函式都屬於此型別）

可透過 typeof 判斷值的型別：
```js
typeof 'Test'  // 'string'
typeof true    // 'boolean'

typeof {}      // 'object'
typeof []      // 'object'
```
## 原始型別的「傳值」（Call By Value）
```js
let num = 5;
let num2 = num;

num = 10;

console.log(num, num2); // 10 5
```
以上面範例來說，變數 `num2` 的值是複製變數 `num` 的值而來，但是將變數 `num` 重新賦值後，變數 `num2` 的值並沒有跟著被改變。

原因是變數 `num` 的值屬於**原始型別**，JS 看到這個原始型別時，會幫變數 `num2` 建立一個新的記憶體空間，並「複製」變數 `num` 的值，最後址派給變數 `num2`，此時兩個變數彼此是獨立的，所以即使變數 `num` 的值改變了，變數 `num2` 也不會受影響，這種情況稱為「傳值」。

## 物件型別的「傳參考」（Call By Reference）
```js
let obj = { val: 5 };
let obj2 = obj;

obj.val = 10;

console.log(obj.val, obj2.val); // 10 10
```
從上述範例可以發現，同樣的行為下，如果換成物件型別，兩個變數的值都會一起被修改。

這是因為 JS 的物件，是透過「記憶體的參考位址」來傳遞資料的，示意圖如下：

![指向圖](https://i.imgur.com/JcXKYmw.png)

當物件 `{ val: 5 }` 指派給變數 `obj` 時，JS 會在記憶體某處建立這個物件，然後再將變數 `obj` 指向存放這個物件的記憶體位址，換句話說，實際上傳入變數 `obj` 裡面的值，是這個記憶體位址。

此時將變數 `obj` 指派給變數 `obj2` 時，變數 `obj2` 所傳入的值，也同樣是這個存放物件 `{ val: 5 }` 的記憶體位址，而因為兩個變數都是指向同一個記憶體位址中的物件，所以當變數 `obj` 重新賦值的同時，變數 `obj2` 的值也會被修改，這種不同變數之間指向同一個記憶體位址的情況，稱為「傳參考」，或是「傳址」。

## 例外情況 / Call By Sharing

```js
let obj = {
  prop: 5
}

function fn(par) {
  par.prop = 500; // 修改屬性
  return par;
}

let obj2 = fn(obj);

console.log(obj, obj2); // { 'prop': 500 } { 'prop': 500 }
console.log(obj === obj2); // true（因為傳參考的關係，因此比較的是記憶體位址，並非存放於記憶體位址中的值）
```
因為作為參數傳入函式的 obj 為物件型別，所以根據傳參考的特性，再函式內修改了屬性內容，會連帶影響到函式外的物件。

但是有一個例外，就是當傳入函式中的物件不是修改屬性內容，而是直接將物件重新賦值時，函式外的物件就不會被影響，範例如下：

```js
let obj = {
  prop: 5
}

function fn(par) {
  par = { // 重新賦值
    prop2: 500
  };
  return par;
}

let obj2 = fn(obj);

console.log(obj, obj2); // { 'prop': 5 } { 'prop2': 500 }
console.log(obj === obj2); // false（重新指向後，兩個變數不再有參考關係）
```
函式外的變數 `obj` 作為參數傳入函式，接著在函式內進行重新賦值的行為，這代表函式內的 `par` 會重新指向一個新物件，而不是指向與函式外的 `obj` 相同的記憶體位址，示意圖如下：

![](https://i.imgur.com/pZvGtHV.png)


以上情況非傳值（Call By Value）、也不屬於傳參考（Call By Reference），因此就衍生出了 Call By Sharing 的說法。

## 小結

- 原始型別指派給變數時，傳遞的是值的複製。
- 物件型別指派給變數時，傳遞的是記憶體的參考位址。
- 傳入函式內的物件，如果重新賦值，此時函式內、外物件之間的參考就會消失。


## 參考資料

[重新認識 JavaScript: Day 05 JavaScript 是「傳值」或「傳址」？](https://ithelp.ithome.com.tw/articles/10191057)

[JS 原力覺醒 Day12- 傳值呼叫、傳址呼叫](https://ithelp.ithome.com.tw/articles/10221506)
