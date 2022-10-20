---
title: JavaScript 觀念 - 函式
date: 2022-09-19
tags:
 - function
categories: JavaScript
index_img: img/banner/banner_js.jpg
---

基本的函式相關筆記。

<!--more-->

------
## 函式（Function）簡述

可以將自訂的程式碼進行包裝，並且能在需要時使用，同時減少撰寫重複的程式碼。此外，函式也能接收一個值（參數），並將接收的值進行運算後再回傳，但也可以選擇不接收、不回傳任何值，以「執行某一段邏輯」的目的存在。

## 函式結構

```js
function doSomeThing(num) {
  return num ** num;
}
```

基本的函式可以使用關鍵字 `function` 來宣告，後方需要加上自訂的函式名稱，透過這種方式宣告的函式稱為「具名函式」，而函式名稱後方的小括號中可放入接收的值（參數），中括號中則是放入想要執行的程式指令，另外可將傳入的值帶入程式中運算，而函式執行後的結果可使用 `return` 決定要回傳的內容，也可以不回傳，但是若有使用 `return` 但是後方沒有接任何要回傳的值，則會回傳 `undefined`。

return 差異如下：

```js
function doSomeThing(num) {
  return num ** num; // 指定回傳的結果
}
doSomeThing(2); // 回傳結果 4
```

```js
function doSomeThing(num) {
  return; // 未指定回傳結果
}
doSomeThing(2); // 回傳結果 undefined
```

## 函式執行

函式在寫好的當下式不會立即執行的，如果要執行，則需要在函式名稱的後方加上一組小括號，這個動作又稱為「呼叫（Invoke）」；而小括號若加在型別非函式的變數後方會產生錯誤，因此不可隨意加上。

執行 / 呼叫函式：

```js
functionName(); // 括號中可決定是否要傳入參數值
```

## 函式終止

函式在被呼叫後，會將函式內的程式碼從第一行執行到最後，接著就會終止函式，但是如果有使用到 `return`，該函式則會提前終止，並回傳 `return` 後方的值。

使用 return 終止函式：

```js
function calculate(num) {
  let result = num + num;
  return result;
  
  result ++; // 提前終止而未被執行
}
calculate(5); // 回傳結果 10
```

## 陳述式與表達式

JavaScript 語法上有區分陳述式、表達式兩種類型。

- 陳述式（Statement）：如同名稱一樣，可想像成在描述一件事情或是對邏輯的描述（如：if 邏輯判斷、具名函式），而陳述式會執行一系列的操作，但是**不會回傳結果**。

  > [MDN 文件 - 陳述式](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators#%E9%81%8B%E7%AE%97%E5%BC%8F)

- 表達式（Expression）：只要程式在執行結束之後**會回傳一個結果**，就屬於表達式，換句話說表達式是一段能被 JavaScript 運算並產生數值的程式碼。

  > [MDN 文件 - 表達式](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators)

陳述 / 表達式範例：

```js
// 變數宣告
var x;
```

```js
// 指派值
x = 5;
```

```js
// 判斷式
if(x === 10) {
  // do something
};
```

可分別將上述程式碼放在開發人員工具 console 中執行查看，變數在宣告後回傳 `undefined`，屬於陳述式；指派一個值給變數，而變數會回傳指派的值，屬於表達式；而判斷式本身並不會回傳任何值，因此屬於陳述式。

## 函式陳述式

前面所提到透過給予函式名稱的所宣告的函式，也就是一般函式宣告，就屬於函式陳述式。

## 函式表達式

除了一般的的函式宣告，還可以將函式指派給一個變數，因為函式本身也屬於物件的一種，因此能夠當作一個被指派的值，而這種方式產生的函式曾為函式表達式。

函式表達式範例：

```js
var expressionFn = function() {
  // do something
}
```

