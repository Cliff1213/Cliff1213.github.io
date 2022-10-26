---
title: JavaScript 筆記 - AJAX
date: 2022-10-24 22:51:28
tags:
 - ajax
categories: JavaScript
index_img: img/banner/banner_data.jpg
---

這是一篇尚未撰寫完成的筆記。

<!--more-->

<div class="toc">

主要內容目錄：

- [XMLHttpRequest](#XMLHttpRequest)
- [jQuery AJAX](#jQuery-AJAX)
- [Fetch](#Fetch)
- [Axios](#Axios)
</div>


## AJAX 簡述

AJAX 是「Asynchronous JavaScript and XML」的縮寫，以名稱來看就是是非同步的 JavaScript 與 XML，可以讓 Web 前端與後端伺服器進行資料的交換，以及不需要重新載入網頁就能更新頁面內容。

過去 AJAX 發出請求會透過 XMLHttpRequest 物件來實作，但是以目前來說，因為有了更適合的方式，因此實務上幾乎不會使用 XMLHttpRequest，不過筆記都寫了，就稍微理解一下每個方法吧！(๑•̀ㅂ•́)و✧

## XMLHttpRequest

### 建立 XHR 物件

```javascript
const xhr = new XMLHttpRequest();
```

此時可以使用 `readyState` 屬性的回傳值來確認狀態。
```javascript
console.log(xhr.readyState); // 0
```
以下是不同回傳結果分別所代表的狀態意思：
* `0`（`UNSENT`）：成功建立一個 XMLHttpRequest 物件，但尚未使用 `open()` 連接伺服器。
* `1`（`OPENED`）：已經使用 `open()` 連接伺服器，但尚未使用 `send()` 發送請求。
* `2`（`HEADERS_RECEIVED`）：已使用 `send()` 發送請求，且可以取得 header 與狀態。
* `3`（`LOADING`）：資料載入中。
* `4`（`DONE`）：資料載入完成，成功接收所有數據。

### open

```javascript
xhrReq.open(method, url, isAsync);
```
連接伺服器，參數依序分別為請求方法（`get`、`post` 等）、資料網址以及是否要非同步執行（預設為 `true`）。

### setRequestHeader

```javascript
xhrReq.setRequestHeader(header, value);
```
設定請求表頭，參數依序分別為表頭名稱與表頭值，以 `post` 請求來說，會用來設定發送主體的資料類型（Content-Type）。

另外，此方法一般會在發送 `post` 請求時呼叫（`get` 請求沒有發送主體因此不使用），程式碼位置會在 `open()` 之後以及 `sned()` 之前。

> 相關內容可參考 [MDN - XMLHttpRequest.setRequestHeader()](https://developer.mozilla.org/zh-TW/docs/Web/API/XMLHttpRequest/setRequestHeader)

> 禁止設定的表頭欄位，請參閱 [MDN - Forbidden header name](https://developer.mozilla.org/en-US/docs/Glossary/Forbidden_header_name)

### send

```javascript
xhrReq.send(body);
```
發送請求，`send()` 的應用會根據請求方法的不同而有所差異，若只是單純的 `get` 請求，參數只需傳入 `null` 即可，其他請求方法如 `post` 則需補上相關資訊。

需要注意的是，資料發送前如果是 JSON 格式，則需要先轉型為字串，轉換型別的方法可以使用 `JSON.stringify()`；同理，接收到的資料如果是字串，也可以透過 `JSON.parse()` 轉換為 JSON 物件。

> 相關資訊請參閱 [MDN - XMLHttpRequest.send()](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/send)


### 非同步執行

雖然可以選擇要以同步或是非同步方式來發出請求，但是為了避免網頁阻塞，一般都會透過非同步方式來執行，而非同步就會衍生出資料尚未載入完畢，程式碼就繼續往下執行的狀況，此時可以透過事件監聽來解決非同步問題。

針對 `load` 事件進行監聽：

```javascript
xhr.addEventListener('load', getData);
function getData() {
  console.log(xhr.responseText, typeof xhr.responseText); // "[{"name":"王小名"}]" string
    
  // 開始操作資料...
  const data = JSON.parse(xhr.responseText); // 將資料結構轉為物件或陣列
  ...
}
```
針對 `xhr` 物件進行 `load` 監聽，此時就能確保資料完全載入後，才執行指定操作，此外，可以發現資料的型別為字串，因此可以視情況將資料轉為物件或陣列使用。

> 字串轉為物件型別可參考 [MDN JSON.parse()](https://https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

### HTTP 狀態碼

資料全部載入後，可以透過 `status` 屬性查看狀態碼。
```javascript
console.log(xhr.status); // 200
```
以下列出常見的狀態碼，以及代表意思：
* 200（請求成功）
* 404（用戶端錯誤）
* 500（伺服器端錯誤）

> 更多相關資訊可參考 [MDN - HTTP 狀態碼](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Status)

### 實作練習


使用[六角學院練習 API](https://hexschool.github.io/ajaxHomework/data.json) 實作 `get` 請求：

```htmlembedded
<p class="txt"></p>
```

```javascript
const url = 'https://hexschool.github.io/ajaxHomework/data.json'; // 資料網址

const xhr = new XMLHttpRequest(); // 建立 XMLHttpRequest 物件
xhr.open('get', url, true); // 連接伺服器
xhr.send(null); // 發送請求

xhr.addEventListener('load', getData); // load 事件監聽
function getData() {
  if(xhr.status == 200) {
    console.log('請求成功');
    
    const data = JSON.parse(xhr.responseText);
  
    const txt = document.querySelector('.txt');
    txt.textContent = data[0].name;
  }else{
    console.log('請求資料有誤');
  }
}
```


使用[六角學院練習 API](https://hexschool-tutorial.herokuapp.com/api/signup) 實作 `post` 請求（註冊功能）：

資料類型：`application/x-www-form-urlencoded`

```javascript
const url = 'https://hexschool-tutorial.herokuapp.com/api/signup'; // 資料網址

const xhr = new XMLHttpRequest(); // 建立 XMLHttpRequest 物件
xhr.open('post', url, true); // 連接伺服器
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded'); // 設定請求表頭
xhr.send('email=test123@gmail.com&password=12345678'); // 發送請求

xhr.addEventListener('load', function() {
  const responseMessage = JSON.parse(xhr.response).message;

  if(xhr.status == 200) {
    console.log('請求成功');
    alert(responseMessage);
  }else{
    console.log('請求資料有誤');
  }
})
```

資料類型：`application/json`

```javascript
const url = "https://hexschool-tutorial.herokuapp.com/api/signup"; // 資料網址

const xhr = new XMLHttpRequest(); // 建立 XMLHttpRequest 物件
xhr.open("post", url, true); // 連接伺服器
xhr.setRequestHeader("Content-type", "application/json"); // 設定請求表頭

const data = {
  email: "test123@gmail.com",
  password: "12345678"
};

xhr.send(JSON.stringify(data)); // 發送請求

xhr.addEventListener("load", function () {
  const responseMessage = JSON.parse(xhr.response).message;

  if(xhr.status == 200) {
    console.log('請求成功');
    alert(responseMessage);
  }else{
    console.log('請求資料有誤');
  }
});
```

## jQuery AJAX

使用前需要引入 jQuery，此處使用 CDN 方式引入。

```htmlembedded
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
```
起手式：

```javascript
$.ajax({
  url:,
  method:,
  dataType:,
  contentType:,
  data:
}).done(function(res) {
  // do something...
}).fail(function(err) {
  // do something...
})
```
以下為常用的參數：

* `url`：請求資料的網址
* `method`：請求方法（`get`、`post`...）
* `data`：傳送的資料
* `dataType`：伺服器接收的資料格式（`text`、`json`...）
* `contentType`：傳送至伺服器的資料格式（預設為 `'application/x-www-form-urlencoded; charset=UTF-8'`）
* `done`：請求成功時的回傳結果
* `fail`：請求失敗時的回傳結果

若是沒有要向伺服器傳送資料（`get` 請求），則參數 `data`、`contentType` 可省略。

jQuery 3.0 版本開始，刪除了 `success()`、`error()` 以及 `complete()` 等方法，取而代之的是 `done()`、`fail()` 以及 `always()`。

>相關內容請參閱[官方文件](https://api.jquery.com/jquery.ajax/#jQuery-ajax-url-settings)說明。

### 實作練習

使用[六角學院練習 API](https://hexschool.github.io/ajaxHomework/data.json) 實作 `get` 請求：

```htmlembedded
<ul class="list"></ul>
```

```javascript
$.ajax({
  url: 'https://hexschool.github.io/ajaxHomework/data.json',
  method: 'get'
}).done(function(res) {
  console.log(res)
    
  $('.list').html(`<li>${res[0].name}</li>`)
}).fail(function(err) {
  console.log(err)
})
```
使用[六角學院練習 API](https://hexschool-tutorial.herokuapp.com/api/signup) 實作 `post` 請求（註冊功能）：

```javascript
$.ajax({
  url: 'https://hexschool-tutorial.herokuapp.com/api/signup',
  method: 'post',
  dataType: 'json',
  contentType: 'application/json; charset=utf-8',
  data: JSON.stringify({
    email: 'test123@mail.com',
    password: '12345678'
  })
}).done(function(res) {
  console.log(res)
    
  alert(res.message)
}).fail(function(err) {
  console.log(err)
})
```

## Fetch

## Axios