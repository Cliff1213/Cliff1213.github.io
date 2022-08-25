---
title: JavaScript 非同步流程處理
date: 2022-08-12
tags:
 - JavaScript
 - 非同步
 - 學習筆記
categories: JavaScript
---

> *非同步筆記。*

<!--more-->

---

## 概念

### 同步執行

JavaScript 程式碼正常的情況下，執行順序都會是前一個的動作執行完後，接續執行下一個動作，這種依序執行的動作就是同步執行，以下方程式碼為例：

```js
// 同步
const btn = document.getElementById('btn');
btn.addEventListener('click', addNum);

function addNum() {
  let result = calcNum(10);
  console.log(result, '2');
}
function calcNum(num) {
  console.log('1')
  return num * num;
}
// 輸出結果
// "1"
// 100 "2"
```

當函式 `addNum()` 被觸發時，會執行 `calcNum()`，此時數值 `10` 會代入參數 `num` 並將計算結果回傳到 `result` 中，最終第 8 行會印出結果  `100`，且從輸出結果也能看出程式碼從觸發到結束，執行順序都有按照設定的邏輯進行。

順序：點擊 `btn` ➔ 觸發函式 `addNum()` ➔ 執行函式 `calcNum()` ➔ 輸出 `1` ➔ 代入參數計算並回傳結果到 `result` ➔ 輸出 `result` 與 `2`。

### 非同步執行
