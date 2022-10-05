---
title: JavaScript 觀念 - 同步與非同步
date: 2022-09-20
tags:
 - synchronous
 - asynchronous
categories: JavaScript
banner:
  url: /images/banner/banner_js.jpg
  width: 1280
  height: 720
---

校正一下自己的觀念。

<!--more-->

------

## JavaScript 中的同步

「同步（Synchronous）」在你的認知上會怎麼解釋？我自己原本的想法是同時處理很多事情，但這個說法在 JavaScript 中卻有些出入，原因在於 JavaScript 是以「單執行緒（單線程）」的方式在執行程序，特性是「一次只做一件事，前面的事做完才會依序做下一件事」。

> 執行緒相關內容可參考此[文章](https://www.itread01.com/ixyfq.html)。

以下方程式碼為例：

```js
function printNum() {
  console.log('1');
} 
printNum();
console.log('2');
```

以上述程式碼來說，會先執行函式 `printNum` 印出 1，接著再印出 2，並且兩者是有先後順序的，屬於同步執行。

---

## 何謂非同步

「非同步（Asynchronous）」的特性則是「可以同時做很多事，並且不需要等待前面的事做完就能做下一件事」，其實就是一開始自己對同步的認知...

以下方程式碼為例：

```js
setTimeout(printNum, 2000);
function printNum() {
  console.log('1');
} 
console.log('2');
```

`setTimeout` 會分別帶入 `callback`、`等待毫秒` 兩個參數，如範例所示，回調函式 `printNum` 被設定 2 秒後才執行，但是在這過程中，排在後方的 `console.log` 並不會等待前面的函式 `printNum`  執行完才動作，而是先印出 2，而 2 秒之後呼叫函式 `printNum` 並印出 1。

...不對呀，JavaScript 屬於單執行緒，所以一次只能做一件事才對，為什麼會有範例中非同步的狀況？原因在於 JavaScript 需要在一些特定的引擎上面才能運行，以 V8（JavaScript 引擎）來說，引擎在運作時是同步的，但是除了引擎內的 JavaScript 原始碼之外，瀏覽器也會提供許多 Web APIs（如：document、MouseEvent、setTimeout...等等），這些並不屬於 V8 引擎的範疇，但是可以透過呼叫並使用它們做到非同步的行為，並且不會影響到 JavaScript 主程式的運行，換句話說，JavaScript 需要依賴 Web APIs 才能達成非同步的效果。

而 `setTimeout` 等待的期間，程式碼為甚麼能夠繼續往下執行？原因是當 Web APIs 與自己寫的 JavaScript 在瀏覽器一起執行時，透過 Web APIs 非同步呼叫的 callback function 會被放到 Callback Queue（儲列 / 佇列）待命，等到其他程式碼都執行完之後才會被回調執行。

---

**參考資料：**

[Javascript 非同步 & Event Loop！10 分鐘輕鬆圖解學習！](https://chanchandev.com/js/Async/async-sync-intro/2534378084/)

[無痛理解 JS | 非同步怎麼運作？](https://5xruby.tw/posts/how-js-synchronous-works)
