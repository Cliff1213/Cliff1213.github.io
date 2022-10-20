---
title: JavaScript 筆記 - 陣列方法
date: 2022-10-08 22:59:09
tags:
 - array
categories: JavaScript
index_img: img/banner/banner_20221008.jpg
---

forEach 很萬用，但其他的也不賴。

<!--more-->

------

> 目錄：
>
> - [forEach](#forEach)
> - [map](#map)
> - [filter](#filter)
> - [find](#find)
> - [every](#every)
> - [some](#some)
> - [reduce](#reduce)

範例資料：

```js
const data = [
  {
    name: 'Leo',
    sex: '男生',
    age: 22
  },{
    name: 'Ryan',
    sex: '男生',
    age: 16
  },{
    name: 'Amber',
    sex: '女生',
    age: 20
  }
]
```



## forEach

forEach 會針對陣列中的每一個元素進行操作。

```js
arr.forEach(function(item, index, array) {
  // do something...
})
```

陣列所元素皆傳入函式執行一次，但是不會回傳任何值（`reutrn` 沒有作用），換言之，forEach 不會產生新陣列。

callback 函式可傳入以下參數：

- `item` 目前元素
- `index` 目前元素索引（選用）
- `array` 陣列本身（選用）

範例：依序印出人物資訊

```js
data.forEach(function(item) {
  console.log(`${item.name} 是${item.sex}，目前 ${item.age} 歲`)
})
// "Leo 是男生，目前 22 歲"
// "Ryan 是男生，目前 16 歲"
// "Amber 是女生，目前 20 歲"
```

## map

map 可以把原陣列所有元素進行轉換，並以新的資料形式在新陣列中呈現。

```js
arr.map(function(item, index, array) {
  // do something...
})
```

會建立一個新陣列，內容為原陣列的每一個元素運算、轉換後回傳的值，若不回傳，新陣列所有元素皆為 `undefined`。

callback 函式可傳入以下參數：

- `item` 目前元素
- `index` 目前元素索引（選用）
- `array` 陣列本身（選用）

範例：將所有人的年齡加五歲

```js
const mapArray = data.map(function(item) {
  return item.age += 5;
})
console.log(mapArray)
// [27,21,25]
```



## filter

filter 就如字面上的意思，可以用來篩選陣列中符合條件的元素。

```js
arr.filter(function(item, index, array) {
  // do something...
})
```

會建立一個新陣列，內容為原陣列中所有符合條件的元素。

callback 函式可傳入以下參數：

- `item` 目前元素
- `index` 目前元素索引（選用）
- `array` 陣列本身（選用）

範例：篩選出年齡 18 歲以上的人

```js
const filterArray = data.filter(function(item) {
  return item.age >= 18;
})
console.log(filterArray);
// [{"name": "Leo", "sex": "男生"},{"name": "Amber", "sex": "女生"}]
```

## find

find 與 filter 類似，差別在於 find 只會找到陣列第一筆符合條件的元素。

```js
arr.find(function(item, index, array) {
  // do something...
})
```

會回傳一個元素，該元素是陣列中第一個符合條件的元素，若都不符合條件則回傳一個 `undefined`。

callback 函式可傳入以下參數：

- `item` 目前元素
- `index` 目前元素索引（選用）
- `array` 陣列本身（選用）

範例：找出第一個女生

```js
const findItem = data.filter(function(item) {
  return item.sex === '女生';
})
console.log(findItem)
// {"name": "Amber", "sex": "女生"}
```

## every

every 可以檢測陣列是否全部元素都符合條件。

```js
arr.every(function(item, index, array) {
  // do something...
})
```

會回傳一個 `boolean`，陣列中的元素必須全部符合條件，最終才會回傳 `true`，否則會回傳 `false`。

callback 函式可傳入以下參數：

- `item` 目前元素
- `index` 目前元素索引（選用）
- `array` 陣列本身（選用）

範例：	判斷是否全部都是男生

```js
const result = data.every(function(item) {
  return item.sex === '男生';
})
console.log(result); // false
```

## some

some 與 every 概念類似，差別在於 some 用來檢測陣列是否有任何一個（或以上）元素符合條件，兩個方法從字面上也可以區別。

```js
arr.some(function(item, index, array) {
  // do something...
})
```

最終會回傳一個 `boolean`，陣列中只要有任何一個元素符合條件，最終就回傳 `true`，否則回傳 `false`。

callback 函式可傳入以下參數：

- `item` 目前元素
- `index` 目前元素索引（選用）
- `array` 陣列本身（選用）

範例：	判斷是否有未成年的人

```js
const result = data.some(function(item) {
  return item.age < 18;
})
console.log(result); // true
```

## reduce

reduce 與其他陣列方法差異較大，可以將陣列中所有元素累計運算，最終回傳一個累計的結果。

```js
arr.reduce(function(accumulator, currentValue, currentIndex, array) {
  // do something...
}, initialValue)
```

接收一個 callback 函式與一個起始值作為參數，callback 函式可傳入以下參數：

- `accumulator` 前一個元素
- `currentValue` 目前元素
- `currentIndex` 目前元素索引（選用）
- `array` 陣列本身（選用）

第二個參數為起始值 `initialValue`（選用），若沒有給定起始值，`accumulator` 預設會是陣列第一個元素，`currentValue` 則預設是第二個；反之，如果有給定起始值，`accumulator` 就會是給定的起始值，而 `currentValue` 會是陣列第一個元素。

callback 函式每次呼叫時，會把 `accumulator` 與 `currentValue` 相加，再把相加的值再次回傳入 `accumulator`， 不斷反覆進行累計，換言之，`accumulator` 除了起始值之外，每次運算的值都是前一次相加累計的值。

範例一：不指定起始值

```js
const arr = [1, 2, 3, 4, 5];
const totalNum = arr.reduce(function(acc, cur) {
  return acc + cur;
})
console.log(totalNum); // 15
// 累加過程：1+2=3 -> 3+3=6 -> 6+4=10 -> 10+5=15
```

範例二：指定起始值為 10

```js
const arr = [1, 2, 3, 4, 5];
const totalNum = arr.reduce(function(acc, cur) {
  return acc + cur;
}, 10)
console.log(totalNum); // 25
// 累加過程：10+1=11 -> 11+2=13 -> 13+3=16 -> 16+4=20 -> 20+5=25
```

範例三：加總範例資料中所有人的年齡

```js
const ageTotal = data.reduce(function(acc, cur) {
  return acc + cur.age;
}, 0)
console.log(ageTotal); // 58
// 累加過程：0+22=22 -> 22+16=38 -> 38+20=58
```

## 