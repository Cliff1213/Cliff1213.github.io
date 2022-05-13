---
title: JavaScript 學習筆記
date: 2022-05-13
tags:
 - JavaScript
 - 學習筆記
categories: JavaScript
---

這段期間開始重溫 JS，為了加深相關基本觀念，因此將學習過程中所撰寫的筆記紀錄於此，並提供後續的資料查詢。

> 本篇筆記會陸續增加內容。

<!--more-->

---

## 宣告

### let

宣告的值為區域變數，可重新賦予值

```javascript
// 宣告一變數 "price"，並賦予值為 60
let price = 60;
// 重新賦予變數 "price" 值為 80
price = 80;
```

### const

宣告的值為常數，僅可讀取不可重新賦予值

```javascript
// 宣告一變數 "sunNum" 並賦予值為 1，該變數值無法在重新賦予
const sunNum = 1;
```

### var

宣告的值為全域變數，可重新賦予值

> 宣告變數的名稱若撞到部分保留字，會無法宣告，保留字可參考此[連結](http://www.w3bai.com/zh-TW/js/js_reserved.html)。

---

## 型別

字串（String）、數字（Number）、布林值（Boolean）...等，可透過 `typeof` 來判斷變數型別，範例如下：

```js
// 範例一
let num = 10;
console.log(typeof num);
// 輸出結果為 Number
```

```js
// 範例二
let num = '10';
console.log(typeof num);
// 輸出結果為 String
```

> 更多型別內容可參考此[連結](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Data_structures)。

### 數字（Number）

只要是數字，並且沒有使用單、雙引號的值，型別都屬於數字，範例如下：

```js
let num = 10;
console.log( typeof num );
// 輸出結果為 Number
```

#### NaN

當兩變數無法進行計算時，會出現 `NaN` 的結果，產生時機範例如下：

```js
let a = 10;
let b = 'hello';
let c = a * b;
console.log( c, typeof c );
// 輸出結果為 NaN number
```

> NaN 主要告知輸出結果有異常，而型別屬於 number。

### 字串（String）

#### 字串相加

字串相加寫法如下：

```js
// 寫法一
let text = 'hello ' + 'world'; // 變數值中帶有半形空白
// 此時變數 text 的結果為 hello world
```

```js
// 寫法二
let name = 'Cliff';
let word = 'Hello!';
console.log( content + ' ' + name ); // 使用 ' ' 取代寫法一的半形空白
// 此時輸出結果為 Hello! Cliff
```

```js
// 寫法三
let name = 'Cliff';
let word = 'Hello!';
let content = name + ' ' + word;
console.log( content );
// 此時輸出結果為 Hello! Cliff
```

寫法一在變數中使用空白，但實務上會比較建議使用寫法二、三的做法。

#### 字串長度

可使用 `length` 來查詢字串的長度，範例如下：

```js
let message = ' Hello! ';
console.log( message.length );
// 輸出結果為 9
```

上述透過 `length` 所計算出來的結果，會包含空白，因此若希望輸出結果自動過濾左右兩側的空白，可使用 `trim()` 語法，範例如下：

```js
let message = ' Hello ! ';
console.log( message.trim().length );
// 輸出結果為 7
```

#### 自動轉型

因為 Javascript 屬於弱型別，因此有些情況下，變數的型別會被自動轉型，範例如下：

```js
let str = '10';
let num = 5;
let mixBoth = str + num;
console.log( mixboth, typeof mixboth );
// 輸出結果為 105 string
```

上述範例中，變數 `str` 與 `num` 兩變數值的型別分別屬於字串與數字，而變數 `mixboth` 將兩者相加，從最後輸出的結果中可以發現，相加的過程中，變數 `num` 的值被自動轉型為字串。

#### 手動轉型

**字串轉數字**

可使用 `parseInt()` 將型別 string 轉為型別 number，範例如下：

```js
let str = '10';
let num = 5;
let mixBoth = parseInt(str) + num;
console.log( mixboth, typeof mixboth );
// 輸出結果為 15 number
```

因為字串與數字相加後的型別會是字串，因此若要正確計算上述兩變數值時，需要透過 `parseInt()` 將變數 `str` 的值型別轉為數字。

> `parseInt()` 使用的時機像是需要取出 `input[type=text]` 表單內容時。

如果變數的值為非數字的字串，則無法正確轉型，範例如下：

```js
let num = parseInt('hello');
console.log( num, typeof num );
// 輸出結果為 NaN number
```

**數字轉字串**

可使用 `toString()` 將型別 number 轉為型別 string，範例如下：

```js
let num = 10;
let num2 = 20;
num2 = num2.toString(); // 將原本的型別 Number 轉為 String，並重新賦予值
let mixBoth = num + num2;
console.log( mixBoth, typeof mixBoth );
// 輸出結果為 1020 string
```

> `toString()` 使用的時機像是需要將電話號碼的區域碼，與後方的號碼做分隔時，就會需要字串與字串相加。

#### value 指向

```js
// 範例一
let email = 'test@mail.com';
email.length;
console.log( email );
// 輸出結果為 test@mail.com
```

上述範例最終輸出結果為 `test@mail.com` 而非 `13`，因為 `length` 只是計算變數的長度，並沒有實際賦予變數一個新值，因此變數 `email` 的值並沒有改變指向。

```js
// 範例二
let email = ' test@mail.com '; // 兩側加入空白
let emailLength = email.length;
email.trim();
console.log( email, emailLength );
// 輸出結果為 ' test@mail.com ' 15
```

上述範例分別宣告了 `email`、`emailLength` 兩個變數並賦予值，而第四行雖然執行了 `trim()` 來過濾變數 `email` 兩側的空白，但並沒有使用 `=` 符號重新賦予該變數新的值，因此最終輸出結果不變，如果希望輸出的值不包含兩側空白，範例如下：

```js
let email = ' test@mail.com '; // 兩側加入空白
let emailLength = email.length;
let updateEmail = email.trim();
email.trim();
console.log( email, emailLength, updateEmail );
// 輸出結果為 ' test@mail.com ' 15 'test@mail.com'
```

上述宣告一變數 `updateEmail` 並賦予值為 `email.trim()`，因此最終輸出結果為 `'test@mail.com'`。

#### 樣板字面值

```js
let name = 'Cliff';
let age = 20;
let content = '我是'+name+'，今年'+age+'歲。';
console.log( content );
// 輸出結果為 '我是Cliff，今年20歲。'
```

上述變數 `content` 的值雖透過 `+` 符號來組成一段文字內容，但這種方式較為麻煩且閱讀起來較困難，因此建議使用樣板字面值，範例如下：

```js
let name = 'Cliff';
let age = 20;
let content = `我是${name}，今年${age}歲。`;
console.log( content );
// 輸出結果為 '我是Cliff，今年20歲。'
```

> 樣板字面值會使用兩個反引號（tab 上方按鍵）符號將內容包覆在其中，而變數使用 `${ 變數名稱 }` 方式插入。

### 布林值（Boolean）

```js
let isSingle = true;
console.log( isSingle, typeof isSingle );
// 輸出結果為 true 'boolean'
```

上述宣告一變數 `isSingle` 並賦予值 `true`，最終輸出型別為 `boolean`（布林值），與數字、字串型別不同的是，布林值只有 `true`、`false` 兩個值，通常用於判斷是或不是。

判斷 100⁹⁹ 與 99¹⁰⁰ 兩數值大小，如下所示：

```js
console.log( 100**99 > 99**100 );
// 輸出結果為 false
```

### undefined

除了前面提到的三種型別之外，較常見的還有 `undefined`，當宣告的變數未賦予（使用 `=` 符號）一個實際的值時，該變數的值與型別就會是 `undefined`，範例如下：

```js
let a;
console.log( a, type a );
// 輸出結果為 undefined 'undefined'
```

> `undefinde` 表示該變數 "尚未被賦予值"。

### null

`null` 表示該變數有被賦予一個值，但是是屬於空值，簡單範例如下：

```js
let a = 10;
a = null;
console.log(a, typeof a);
// 輸出結果為 null 'object'
```

> `null` 通常會在需要清空物件或陣列中的資料內容時使用。

---

## 運算子

> 補充知識：流程圖的形狀代表意義，橢圓 = 流程起點 / 終點，矩形 = 處理流程，菱形 = 判斷是否。

### 比較運算子

常見的比較運算子如下所示：

```txt
| > 大於 | < 小於 | >= 大於且等於 | <= 小於且等於 | == 等於 | != 不等於 | === 等於（包含型別） | !== 不等於（包含型別） |
```

比較運算子所運算的結果會以布林值（Boolean）表示，如下所示：

```js
let a = 50;
let b = 10;
let c = a > b;
console.log( a > b );
// 輸出結果為 true
console.log( ( b**2 ) > a );
// 輸出結果為 true
console.log( c );
// 變數 c 的值會先在記憶體中計算出結果，再回傳計算後的結果 true 並賦予變數值，因此最終輸出結果為 true
```

#### 比較運算子補充觀念

**觀念一**

`=` 與 `==` 兩者在作用上的差異在於前者表示 "賦予" 一個變數值，後者則是判斷兩變數的值 "是否相等"。

**觀念二**

`==` 與 `===` 都是用來判斷兩變數的值是否相等，但有些許差異，差異如下：

```js
let a = 10;
let b = '10';
console.log( a == b );
// 輸出結果為 true
console.log( a === b );
// 輸出結果為 false
```

從上述範例可以得知，`==` 在比較的過程中如果內容相同但型別不同，就會自動轉型變數的型別，而 `===` 除了比較內容之外，也會判斷兩變數的型別是否相等，因此後者較為嚴謹。

> 上述範例同理 `!=` 與 `!==`。

### 邏輯運算子

邏輯運算子會使用的符號為 `&&`（and）、`||`（or），分別表示 "同時滿足條件"、"滿足其中一個條件" ，範例如下：

```js
let a = 50;
let b = 10;
let c = 100;
console.log( a >= b && a > c ); // a >= b 且 a < c / 兩條件皆必須為 true
// 輸出結果為 false
console.log( a >= b || a > c ); // a >= b 或 a < c / 其中一個條件為 true 即可
// 輸出結果為 true
```

> 邏輯運算子常見的情境像是百貨公司周年慶 e.g 同款項第二件九折，若單筆消費超過兩千元，則送百元折價券。

邏輯運算子也可以用來判斷兩個以上的條件，範例如下：

```js
let sex = 'male';
let age = 18;
let isSingle = false;
console.log( sex == 'male' && age >= 18 && isSingle == true );
// 不滿足 isSingle == true 條件，因此輸出結果為 false
```

### 賦值運算子

賦值運算子會使用 `+=`, `-=` 等方式來變更變數值，以下範例為變更一個變數值的方法：

```js
let wallet = 500; // 錢包總共有 500 元
wallect = 400; // 買了 100 元的早餐，剩餘 400 元
wallet = 450; // 路上撿到 50 元，現在總共有 450 元
console.log( wallect );
// 輸出結果為 450
```

上述範例若使用賦值運算子，寫法如下：

```js
let wallet = 500; // 錢包總共有 500 元
wallect = wallet - 100; // 買了 100 元的早餐，剩餘 400 元
wallect = wallect + 50;
console.log( wallect );
// 輸出結果為 450
```

如上述情境所示，透過賦值運算子來計算花費的金額，邏輯與可辨識性相較前者高。

#### 賦值運算子縮寫

```js
let num = 1;
num = num + 1;
// num 最終的值為 2
```

將上述範例使用縮寫撰寫，如下所示：

```javascript
// 縮寫
let num = 1;
num += 1;
// num 最終的值為 2
```

若只需要針對變數值進行 `+=1` 或 `-=1` 的動作，可以使用 `++` 或 `--`，範例如下：

```javascript
let a = 1;
let b = 2;
a++;
b--;
console.log( a, b );
// 輸出結果為 2, 1
```

雖然 `++` 本身沒有 `=` 符號，但也會重新賦予變數值，因此上述三種方式的運作方式與結果皆相同。

> 更多相關運算子內容可參考此[連結](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Expressions_and_Operators)。

