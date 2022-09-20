---
title: JavaScript 觀念 - 同步與非同步
date: 2022-09-20
tags:
 - synchronous
 - asynchronous
categories: JavaScript
---

JavaScript 觀念筆記。

<!--more-->

------

## JavaScript 中的同步（synchronous）

「同步」在你的認知上會怎麼解釋？我自己原本的想法是同時處理很多事情，但這個說法在 JavaScript 中卻有些出入，原因在於 JavaScript 是以「單執行緒（單線程）」的方式在執行程序，特性是「一次只做一件事，前面的事做完才會依序做下一件事」。

以下方程式碼為例：

```js
function showMsg() {
  console.log('1');
} 
showMsg();
console.log('2');
```

以上述程式碼來說，會先執行函式 `showMsg()` 印出 1，接著再印出 2，並且兩者是有先後順序的，屬於同步執行。

---

## 何謂非同步（Asynchronous）

非同步的特性則是「可以同時做很多事，並且不需要等待前面的事做完就能做下一件事」，其實就是一開始自己對同步的認知...

以下方程式碼為例：

```js
setTimeout(showMsg, 2000);
function showMsg() {
  console.log('1');
} 
console.log('2');
```

`setTimeout()` 會分別帶入 `callback`、`等待毫秒` 兩個參數，如範例所示，回調函式 `showMsg` 被設定 2 秒後才執行，但是在這過程中，排在後方的 `console.log` 並不會等待前面的函式 `showMsg`  執行完才動作，而是先印出 2，而 2 秒之後呼叫函式 `showMsg` 並印出 1。

...不對呀，JavaScript 屬於單執行緒，所以一次只能做一件事才對，為什麼會有範例中非同步的狀況？原因在於 JavaScript 需要在一些特定的引擎上面才能運行，以 V8（JavaScript 引擎）來說，引擎在運作時是同步的，但是除了引擎之外，瀏覽器也會提供許多 Web APIs（如：document、MouseEvent、setTimeout...等等），這些並不屬於 V8 引擎的範疇，但是可以透過呼叫來使用並完成非同步的事件處理，換句話說，JavaScript 需要依賴 Web APIs 才能打到非同步的效果。

---

**參考資料：**

[如何理解單執行緒、多執行緒？如何選擇多執行緒、多程序？](https://www.itread01.com/ixyfq.html)

[無痛理解 JS | 非同步怎麼運作？](https://5xruby.tw/posts/how-js-synchronous-works)
