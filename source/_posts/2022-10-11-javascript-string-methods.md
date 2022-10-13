---
title: JavaScript 筆記 - 字串方法
date: 2022-10-11 14:13:04
tags:
 - string
categories: JavaScript
banner:
  url: /images/banner/banner_20221011.jpg
  width: 1280
  height: 720
---

記錄字串處理的方法。

<!--more-->

------

> 目錄：
>
> - 取得字串字數：[length](#length)
> - 字串分割：[split](#split)
> - 字串連接：[concat](#concat)
> - 尋找文字索引：[indexOf](#indexOf)
> - 替換文字內容：[replace](#replace)
> - 匹配文字：[match](#match)
> - 取得兩個索引之間的所有文字：[substring](#substring)、[slice](#slice)
> - 轉換文字大小寫：[toUpperCase / toLowerCase](#toUpperCase-toLowerCase)

## length

可以取得字串的字數。

```js
str.length
```

範例：取得字串字數

```js
let str = 'hello world';
let strLength = str.length;
console.log(strLength); // 11
```

## split

可以將字串分割，並將被分割的字串依序存入一個新陣列中。

```js
str.split(separator)
```

參數 `separator` 表示指定的分割字符（依據甚麼內容做分割）。

範例一：以半形空白做分割

```js
let str = "hello world";
const arr = str.split(" ");
console.log(arr); // ["hello", "world"]
```

範例二：以空字串做分割

```js
let str = "hello world";
const arr = str.split("");
console.log(arr); // ["h","e","l","l","o"," ","w","o","r","l","d"]
```

## concat

可以連接多個字串，並且不會影響原字串，如果連接的內容的型別不是字串，則會轉型為字串再連接。

```js
str.concat(str2, ...strN)
```

參數 `str2`、`...strN` 表示要連接的字串，會被連接在原字串後方。

範例一：連接字串

```js
let str = 'hello';
let concatStr = str.concat(' ','world');
console.log(concatStr); // "hello world"
```

範例二：連接非字串型別

```js
let str = '123';
let concatStr = str.concat(456);
console.log(concatStr); // "123456"
```

> [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/concat) 文件中提到，建議使用 +、+= 來取代 concat()。

## indexOf

可以尋找字串中指定的字串位置，只會尋找第一個符合的字串，若找不到則回傳 `-1`。

```js
str.indexOf(searchValue, position)
```

參數 `searchValue` 表示要搜尋的字串內容，`position` 則是起點的索引（選用）。

範例一：尋找字串中 "e" 索引位置

```js
let str = 'hello world';
let strIndex = str.indexOf('e');
console.log(strIndex); // 1
```

範例二：尋找指定內容

```js
let str = 'hello world';
let strIndex = str.indexOf('world');
console.log(strIndex); // 6
```

範例三：找不到指定內容

```js
let str = 'hello world';
let strIndex = str.indexOf('a');
console.log(strIndex); // -1
```

範例四：從索引 3 開始尋找字串中 "e" 的位置

```js
let str = 'hello world';
let strIndex = str.indexOf('e', 3);
console.log(strIndex); // -1（起點位置為第 4 個字，因此找不到第二個字 e）
```

## replace

可以將字串內容替換成指定的字串。

```js
str.replace(oriStr, newStr)
```

參數 `oriStr` 表示需要做替換的內容，一般情況下只能取代第一個符合的字串，如果要全部替換，需要搭配正規表達式，`newStr` 則表示替換後的內容。

範例一：將字串中的第一個 "o" 替換為 "i"

```js
let str = 'hello world';
let newStr = str.replace('o', 'i');
console.log(newStr); // "helli world"
```

範例二：搭配正規表達式，將字串中所有 "o" 替換為 "i"

```js
let str = 'hello world';
let newStr = str.replace(/o/g, 'i');
console.log(newStr); // "helli wirld"
```

> 若需要全部取代，除了使用正規表達式之外，也可以透過 replaceAll() 方法來達成。

## match

會回傳「匹配到的字串內容」，為一個陣列，若找不到匹配的字串，則回傳 `null`。

```js
str.match(regexp)
```

參數 `regexp` 表示想要匹配的字串內容，通常會搭配正規表達式，若使用 `g` 字符，回傳的陣列內容為所有匹配到的字串，反之，則只有第一個匹配到的字串，並且帶有附加屬性。

範例一：不使用正規表達式

```js
let str = 'hello world';
let newStr = str.match('l');
console.log(newStr); // ['l', index: 2, input: 'hello world', groups: undefined]
```

範例二：搭配正規表達式（不使用 `g` 字符）

```js
let str = 'hello world';
let newStr = str.match(/l/);
console.log(newStr); // ['l', index: 2, input: 'hello world', groups: undefined]
```

範例三：搭配正規表達式（使用 `g` 字符）

```js
let str = 'hello world';
let newStr = str.match(/l/g);
console.log(newStr); // ['l', 'l', 'l']
```

## substring

可以取得字串中，兩個索引位置之間的文字內容。

```js
str.substring(indexStart, indexEnd)
```

參數 `indexStart` 表示起始索引，`indexEnd` 則是結束索引，若不使用結束索引，範圍則會是起始索引到最後一個字之間，此外，起始、結束索引對調不影響結果。

範例一：取出索引位置 2 到 8 之間的文字

```js
let str = 'hello world';
let newStr = str.substring(2, 8);
console.log(newStr); // "llo wo"
```

範例二：取得索引位置 6 到最後一個字之間的文字

```js
let str = 'hello world';
let newStr = str.substring(6);
console.log(newStr); // "world"
```

## slice

作用與 `substring` 類似，差別在於 `slice` 參數可以傳入負數。

```js
str.slice(indexStart, indexEnd)
```

參數 `indexStart` 表示起始索引，若為負值，`indexEnd` 則是結束索引，若不使用結束索引，範圍則會是起始索引到最後一個字之間，若使用負值，則索引順序會從字串最後一個字往回計算。

範例一：取得索引位置 2 到 8 之間的文字

```js
let str = 'hello world';
let newStr = str.substring(2, 8);
console.log(newStr); // "llo wo"
```

範例二：取得倒數第 5 個字，到最後一個字之間的文字

```js
let str = 'hello world';
let newStr = str.slice(-5);
console.log(newStr); // "world"
```

## toUpperCase / toLowerCase

可以將字串中的字母轉換為大、小寫

```js
str.toUpperCase() // 轉換為大寫
str.toLowerCase() // 轉換為小寫
```

範例一：轉換為大寫

```js
let str = 'hello world';
let upperCaseStr = str.toUpperCase();
console.log(upperCaseStr); // "HELLO WORLD"
```

範例二：轉換為小寫

```js
let str = 'HELLO WORLD';
let lowerCaseStr = str.toLowerCase();
console.log(lowerCaseStr); // "hello world"
```

