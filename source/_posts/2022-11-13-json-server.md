---
title: 初見 JSON Server
date: 2022-11-13 10:32:49
tags:
 - JSON Server
 - RESTful API
categories: Package
index_img: img/banner/banner_npm.jpg
---

使用 JSON Server 建立 RESTful API。
<!--more-->

---
<div class="toc">
<p class="toc-title">目錄：</p>

- [前言](#前言)
- [RESTful API 基本概念](#RESTful-API-基本概念)
- [安裝 JSON Server](安裝-JSON-Server)
- [建立資料庫](#建立資料庫)
- [JSON Server 注意事項](#JSON-Server-注意事項)
- [發送請求（axios 為例）](#發送請求（axios-為例）)
- [GET 取得資料](#GET-取得資料)
- [指定 Port](#指定-Port)
- [新增靜態網頁](#新增靜態網頁)
- [資料操作](#資料操作)
</div>

## 前言

[JSON Server](https://github.com/typicode/json-server) 可以快速建立一個虛擬的資料庫，很適合前端菜鳥用於作品集，但是實務上正式的資料庫則完全不建議用此方法來建立。

## RESTful API 基本概念

REST 完整名稱 Representational State Transfer（表現層狀態轉移），只是一種設計風格，RESTful 只是轉換成形容詞，而 RESTful API 可以理解成以 REST 規範所設計的 API，實際上並沒有任何特殊功能。

以使用者資料來說，一般的 API 可能為以下形式：
```
取得全部使用者 GET     api/users/getAll
取得指定使用者 GET     api/users/getUser/1
新增一筆使用者 POST    api/users/createUser
更新指定使用者 POST    api/users/updateOne
刪除指定使用者 DELETE  api/users/deleteUser/1
```
若是依照 RESTful API 規範則是以下形式：

```
取得全部使用者 GET     api/users
取得指定使用者 GET     api/users/1
新增一筆使用者 POST    api/users
更新指定使用者 PUT     api/users/1
刪除指定使用者 DELETE  api/users/1
```
上述範例可以看到 RESTful API 都是使用相同網址路徑，只是請求的方法不同，換句話說，只需要從請求方法就能得知這個 API 是要進行 C、R、U、D 哪種操作，RESTful API 因為網址更一致，不同開發者維護上也會更清楚。

>CRUD 分別代表 Create、Read、Update、Delete。

## 安裝 JSON Server


以下使用 npm 進行全域安裝：

```tex
npm install -g json-server
```

> 基本的 Node.js 環境本篇不討論

## 建立資料庫

手動新增一個 JSON 檔案作為資料庫，如以下範例：

```json
{
  "users": [
    {
      "name": "Jack",
      "id": 1
    }, {
      "name": "Leo",
      "id": 2
    }, {
      "name": "Annie",
      "id": 3
    }
  ]
}
```
>需要注意 JSON 格式的最外層必須是物件，並且屬性與對應的值，需要使用雙引號包覆。

完成資料庫的建立後，即可輸入以下指令開啟 JSON Server：

```tex
json-server --watch data.json
```
指令中 `data` 為 JSON 檔案名稱（需要注意路徑是否正確），此外，如果不先建立檔案直接輸入指令，預設會產生一個 JSON 檔案，以下是預設的資料內容：

```json
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```
接著成功開啟 JSON Server 之後，終端機會回應以下資訊：

![](https://i.imgur.com/85TEwvP.png)

`Resources` 會對應 JSON 物件下的每個屬性，`Home` 下方的 `localhost:3000` 為伺服器的路徑，嘗試從瀏覽器網址列輸入該路徑就會進入預設的首頁：

![](https://i.imgur.com/W1eiz2E.png)

頁面中 Resources 下方可以看到 `/users` 的連結，進入連結後會看到一開始新增的資料（下圖），而這個網址就是我們能操作的 RESTful API。

![](https://i.imgur.com/mPb5kqL.png)

## JSON Server 注意事項

- Content-Type 只支援 applicetion/json
- 所有 `POST`、`PUT`、`PATCH`、`DELETE` 操作都會自動儲存 json 檔案，建議將原資料先備份
- 預設支援 CORS 與 JSONP 協定（對所有來源開放）
- 每筆資料都會有 `id` 屬性，該屬性新增後無法再變更
- 終端機輸入 `s` 並按下 Enter 會建立一個 json 檔案，內容與黨前資料庫相同

## 發送請求（axios 為例）

常用的請求方法有以下幾種：

- GET：取得資料
- POST：新增資料
- PUT：修改完整資料
- PATCH：修改局部資料
- DELETE：刪除資料


### GET 取得資料

```javascript
axios('http://localhost:3000/users')
  .then(function(res) {
    console.log(res)
  })
  .catch(function(err) {
    console.log(err)  
  })
```
![](https://i.imgur.com/kt1gKua.png)

可以看到回傳結果 `data` 資料與 JSON Server 內容相同。

### POST 新增資料

```htmlembedded
<input type="text" placeholder="輸入名稱" id="userName">
<input type="button" value="新增使用者資料" id="createBtn">
```

```javascript
const userName = document.querySelector('#userName');
addUserBtn.addEventListener('click', function() {
  const addUserBtn = document.querySelector('#createBtn');
  axios({
    url: 'http://localhost:3000/users',
    method: 'post',
    data: {
      name: userName.value
    }
  }).then(function(res) {
    console.log(res)
  }).catch(function(err) {
    console.log(err)
  })
})
```

當點擊 `新增使用者資料` 按鈕之後，進入 `http://localhost:3000/users` 可以看到資料新增進去了（下圖），也可以開啟 data.json 檔案，結果會相同。

![](https://i.imgur.com/bYHat3x.png)

### PUT 修改資料

```htmlembedded
<input type="text" placeholder="輸入修改 ID" id="userId">
<input type="text" placeholder="輸入修改名稱" id="userName">
<input type="button" value="更新使用者資料" id="updateBtn">
```

```javascript
const updataBtn = document.querySelector('#updateBtn');
updateBtn.addEventListener('click', function() {
  const userId = document.querySelector('#userId');
  const userName = document.querySelector('#userName');
  axios({
    url: `http://localhost:3000/users/${userId.value}`,
    method: 'put',
    data: {
      name: userName.value
    }
  }).then(function(res) {
    console.log(res)
  }).catch(function(err) {
    console.log(err)
  })
})
```

### DELETE 刪除資料

```htmlembedded
<input type="text" placeholder="輸入刪除IDID" id="deleteId">
<input type="button" value="刪除使用者資料" id="deleteBtn">
```

```javascript
const deleteBtn = document.querySelector('#deleteBtn');
deleteBtn.addEventListener('click', function() {
  const userId = document.querySelector('#userId');
  axios({
    url: `http://localhost:3000/users/${userId.value}`,
    method: 'delete'
  }).then(function(res) {
    console.log(res)
  }).catch(function(err) {
    console.log(err)
  })
})
```

輸入 `2` 並點擊 `刪除使用者資料` 按鈕之後，`id` 為 `2` 的資料就被刪除了（下圖）。

![](https://i.imgur.com/MBohFtZ.png)

## 指定 Port

JSON Server 預設使用 `localhost:3000`，若有變更需求可以在終端機入以下指令：

```tex
json-server --watch xxx.json --port 指定的號碼
```

## 新增靜態網頁

只需要在資料夾目錄建立一個 public 資料夾，再將所有靜態檔案放入資料夾裡面即可，而預設的首頁檔名為 `index.html`。

## 資料操作

透過網址也能對資料進行各種操作：

- 搜索資料（全部內容）

```tex
http://localhost:3000/users?q=Cliff
```

- 篩選資料（指定內容）

```tex
http://localhost:3000/users?name="Cliff"
```

- 資料分頁

```tex
http://localhost:3000/users?_page=5&limit=3
```

- 資料排序

```tex
http://localhost:3000/users?_sort=id&_order=DESC
```

> 更多操作方式請參閱[官方文件](https://github.com/typicode/json-server)。

## 參考資料

[JSON Server Github](https://github.com/typicode/json-server)
[Will 保哥 - 線上直播](https://www.youtube.com/watch?v=uFKa4xrc42c)