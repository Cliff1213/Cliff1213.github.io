---
title: JavaScript 筆記 - 迴圈
date: 2022-10-07 13:19:21
tags:
 - loop
 - note
categories: JavaScript
index_img: img/banner/banner_loop.jpg
---

Looooooop。

<!--more-->

------
迴圈（Loop）指的就是不斷重複做一件事情，當條件滿足就執行一次，一直到不符合條件就終止迴圈，常見的迴圈有 for、while，還有一個 do...while，不過這篇筆記先不提。

說個笑話，廢物如我一直把迴圈跟陣列方法當作是同類型的東西。

## for

範例：

```js
let step = 0;
for(let i = 0; i <= 10; i++) {
  step += i;
  console.log(i); // 0, 1, 2...10
}
console.log(step); // 55
```

for 迴圈小括號中的 `i` 是一個變數，而左到右依序分別代表**初始值**、**條件**以及**結束時的變動**，以範例來說，`i` 從 `0` 開始，如果  `i <= 10` 條件為 true，就執行大括號`{}`裡面的程式碼，執行結束後 `i` 就加 `1`，反覆循環直到條件為 false 時，就跳出迴圈。

使用 for 迴圈需要注意小括號中的變數 `i`，盡可能都使用 `let` 來宣告，如果用 `var` 宣告，則變數 `i` 的作用域就不會在 for 迴圈的大括號裡面。

## while

範例：

```js
let i = 0;
while(i <= 10) {
  console.log(i); // 0, 1, 2...10
  i ++;
}
console.log(i); // 11
```

while 迴圈比較直白，可以將一個變數放入小括號中進行條件判斷，當判斷為 true 時，就執行大括號 `{}` 中的程式碼，而結束時的變動需加入執行的程式碼當中，如大括號中的 `i ++`。

> 無論是使用 for 還是 white，終止迴圈的條件都要多加留意，以避免造成無窮迴圈，無窮迴圈意味著你的迴圈永遠不會終止，以上面 white 迴圈為例，如果不加上 `i ++`，每次判斷都會是 true。

## break 與 continue

雖然迴圈可以重複執行程式碼，但是有時候可能會希望迴圈在達到某個目的時，就終止迴圈而非一路執行到底，此時就可以使用 break 或是 continue，通常會搭配 if 一起使用。

### break

break 能夠直接終止迴圈，以下面程式碼為例：

```js
const numList = [9,5,12,21,7,11,16];
for(let i = 0; i <= numList.length-1; i++) {
  if(i % 3 === 0) {
    console.log(numList[i]);
    break;
  }
}
// 9
```

找出陣列中第一個能夠被 3 整除的數字，找到後終止迴圈。

### continue

continue 可以在執行迴圈的過程中，跳過一些指定特定的條件：

```js
const numList = [9,5,12,21,7,11,16];
const newAry = [];
for(let i = 0; i <= numList.length-1; i++) {
  if(i % 3 === 0) {
    newAry.push(numList[i]);
    continue;
  }
}
console.log(newAry); // [9, 21, 16]
```

將陣列 `numList` 中符合 3 的倍數的項目篩選出來，再依序加入新陣列 `newAry` 中。

---
