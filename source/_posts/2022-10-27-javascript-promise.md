---
title: JavaScript 筆記 - Promise
date: 2022-10-27 09:18:44
tags:
 - promise
categories: JavaScript
index_img: img/banner/banner_promise.jpg
---

在研究 AJAX 時，認識了這個 Promise 新語法，因此決定先停筆，來稍微理解 Promise 的基本的運作方式，內容會慢慢補充完整。

<!--more-->

---
<div class="toc">
<p class="toc-title">目錄：</p>

- [狀態](#狀態)
- [建立 Promise 物件](#建立-Promise-物件)
- [then、catch](#then、catch)
</div>


Promise 是 ES6 新增的語法，用於處理非同步的行為，以白話文的方式說就是「我會幫你做一件事，結果會成功或失敗不確定，但是做完後會把結果告訴你」，換句話說，Promise 可以理解成一個表示成功或失敗的未來值。

## 狀態

一個 Promise 在處理非同步操作的過程中，會有以下幾種狀態：

1. `pending`：操作尚未處理完成，結果待定。
2. `settled`：操作已處理完成，結果已確定，為以下兩種結果之一：
    - `fulfilled`：操作成功，Promise 已被實現。
    - `rejected`：操作失敗，Promise 已被拒絕。


## 建立 Promise 物件
```javascript
new Promise(executor)
```
Promise 是一個建構函式，需要透過 `new` 關鍵字來建立，接收一個函式（`executor`）作為參數參數，用來封裝非同步的操作。

當 Promise 完成非同步操作後，若操作成功（狀態 `fulfilled`），可以呼叫函式 `resolve` 回傳結果，並且可以使用 `then` 來接收結果，反之，若操作失敗（狀態 `rejected`），可以呼叫 `reject` 來回傳結果，並使用 `catch` 來接收結果，而無論成功或失敗只會得到一個結果。

Promise 一般通常會用函式封裝，當需要取得非同步操作結果時，再呼叫該函是即可，範例如下：

```javascript
function fn() {
  return new Promise(function(resolve, reject) {
    // ...
  })
}
fn();

console.log(fn());
```
![](https://i.imgur.com/pTVfu2l.png)

也可以從 `console.log` 看到 Promise 目前的狀態 `PromiseState`，以及等待向外回傳的結果 `PromiseResault`。

以下是簡單的非同步操作範例：

```javascript
function promiseAsyncOperate(status) {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      if(status) {
        resolve('非同步操作成功');
      }else{
        reject('非同步操作失敗');
      }
    }, 1000);
  })
}
promiseAsyncOperate(true) // 模擬非同步操作成功
```
上述建立了一個函式 `promiseAsyncOperate` 來將非同步操作封裝起來，並且以參數 `status` 來模擬非同步操作的結果為成功或失敗，接著執行函式並傳入 `true`。

>實務上非同步操作的成功與否並不是由我們決定的，以 AJAX 來說，當瀏覽器對伺服器發出請求時，可能會因為各種因素而影響到伺服器回傳的結果，範例只是透過模擬來演示過程。

此時函式中的 Promise 就會執行，而因為傳入 `true` 的關係，判斷式中的 `resolve` 會被呼叫，並且回傳非同步操作的結果，以範例來說就是代入的字串內容。

## then、catch

接續上一個範例，若需要將回傳結果進行後續的處理改怎麼做？這個問題前面就有提到，`resolve` 所回傳的結果，可以使用 `then` 來接收，反之，若一開始傳入的值為 `false`，`reject` 就會被呼叫，此時就能使用 `catch` 來接收回傳結果，如下：

```javascript
function promiseAsyncOperate(status) {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      if(status) {
        resolve('非同步操作成功');
      }else{
        reject('非同步操作失敗');
      }
    }, 1000);
  })
}
promiseAsyncOperate(true)
.then(function(res) {
  console.log(res); // 取得　resolve 回傳結果 => '非同步操作成功'
})
.catch(function(err) { // 非同步操作成功因此未被執行
  console.log(err);
})
```

