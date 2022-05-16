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

可使用 `length` 來查詢變數內容的長度，範例如下：

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

### 比較運算子

常見的比較運算子如下所示：

```tex
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

## if 流程判斷

流程判斷會使用到的判斷方式有 `if`、`else if`、`else` 三種，分別可以理解成 "如果"、"或是"、"否則"，如下方範例情境所示：

**情境一**

```js
// 情境：判斷是否下雨，若是，則待在家不出門
let isRain = true;
if( isRain == true ){ // 判斷結果為 true
    console.log('待在家不出門');
}
// 輸出結果為 '待在家不出門'
```

上述範例中，`if` 右方小括弧中的內容稱為條件式，若條件式的判斷結果為 `true`（滿足條件式），則執行大括弧中的內容。

**情境二**

```js
// 情境：判斷是否消費滿 1000 元，若是，則打九折，若否，則不打折
let cost = 800;
if( cost >= 1000 ){
    console.log('消費滿千，可以折扣');
} else {
    console.log('未滿千元，沒有折扣');
}
// 輸出結果為 '未滿千元，沒有折扣'
```

上述範例中，因為條件式 `cost >= 1000` 判斷結果為 `false`，因此跳過第 4 行的內容而執行 `else` 的內容。

> 若 `if` 判斷的結果為 `false`，但沒有給予對應的 `else` 內容，就會終止判斷且不會執行任何內容。

**情境三**

```js
// 情境：判斷錢包的錢是否夠買麵配可樂
let wallet = 110;
let noodlePrice = 120;
let cokePrice = 40;
if( wallet >= ( noodlePrice + cokePrice ) ){
    console.log('午餐吃麵配可樂，真爽');
}else if( wallet > noodlePrice && wallet < ( noodlePrice + cokePrice ) ){
    console.log('午餐只能吃麵');
}else {
    console.log('午餐只喝可樂，e04');
}
```

**情境四**

```js
// 情境：今天計畫要出門，如果下毛毛雨就帶輕便雨衣，如果下小雨就帶傘，但如果下豪雨就不出門。
let weather = '小雨';
console.log(`現在外面${weather}，所以`);
if( weather == '無雨' ){
    console.log('不需要攜帶雨具');
}else if( weather == '毛毛雨' ){
    console.log('攜帶輕便雨衣');
}else if( weather == '小雨' ){
    console.log('攜帶一把傘');
}else if( weather == '小雨' ){
    console.log('攜帶一把傘');
}else if( weather == '豪雨' ){
    console.log('待在家不出門');
}else{
    console.log('狀態異常');
}
// 輸出結果為 現在外面小雨，所以 攜帶一把傘
```

如上述範例所示，`else if` 可以使用多個，而 `if`、`else` 僅可頭尾分別存在一個。

**情境五**

```js
// 情境：判斷考試成績區間
let point;
point = 89;
console.log(point, typeof point)
if( point >= 80 && (typeof point) != undefined ){
    console.log('成績優異');
}else if( 80 > point && point >= 60 && (typeof point) != undefined ){
    console.log('請保持');
}else if( 60 > point && (typeof point) != undefined ){
    console.log('請再加油');
}else{
    console.log('資料有誤');
}
```

> 範例程式碼中，最後的 `else` 主要用於判斷程式碼若不符合前面所有條件時，所產生的異常狀態，有利於除錯。

### 流程圖

初學者在撰寫流程判斷時，若有事先規劃的流程圖，可以有效提高撰寫的效率與程式邏輯思維，流程圖的形狀代表意義如下：

```
箭頭 = 流程走向
橢圓 = 起止符號 = 流程起點/終點
矩形 = 處理流程 = 一系列的程式去改變數值、形式、數據的位置
菱形 = 決策判斷 = 判斷條件，視情況決定下一步走向，通常以是/否決定。
```

> 推薦使用 [Whimsical](https://whimsical.com/) 線上工具來設計流程圖，流程圖參考範例：[範例一](https://i.imgur.com/7KufOX3.png)、[範例二](https://i.imgur.com/zzvsblf.png)。

### if 巢狀運用

if 在判斷條件式後，執行的內容中也能夠放入其他細部判斷式，流程圖可參考此[連結](https://i.imgur.com/g1WaOjc.png)，範例如下：

```js
// 情境：判斷不同性別的體態狀況
// 測試資料
let sex = 'male';
let centimeter = 88;
// 將測用的資料帶入流程判斷
if( sex == 'male' ){
    if( centimeter >= 90 ){
        console.log('男生體態過胖');
    }else{
        console.log('男生體態正常');
    }
}else if( sex == 'woman' ){
    if( centimeter >= 80 ){
        console.log('女生體態過胖');
    }else{
        console.log('女生體態正常');
    }
}else{
    console.log('您輸入的資料有誤');
}
// 輸出結果為 男生體態正常
```

上述範例中，資料首先在第 6 行開始進行判斷式 `sex == 'male'` 的判斷，符合條件後執行下方內容，再執行內容中 `if` 的判斷式 `centimeter >= 90`，判斷後不符合條件，最後執行 `else` 的內容，執行完畢後終止該判斷流程。

> if 只要滿足其中一個條件（判斷式結果為 `true`），就會在執行該條件的內容後，終止判斷流程（不再進行後續判斷式判斷）。

## 資料結構

### 陣列（Array）

一般的情況下，一個變數只能賦予一個值，但是在多筆資料的狀況下，難以針對每一個變數去賦予對應的值，因此在處理多筆資料時，通常會將這些資料透過一個陣列來表示，並賦予到一個變數中，範例如下：

```js
// 資料可以是各種型態
let data = ['red', 'yellow', 'green'];
let data = [30, 12, 15, 20];
let data = [true, false, false];
let data = ['red', 30, true];
let data = []; // 表示沒有資料的空陣列
```

> 陣列中的所有資料會放入一個中括號內，且每筆資料會使用半形逗號隔開。

#### 取得陣列資料

進行抓取指定資料前，需要先理解陣列中的每筆內容是有順序排列的，而第一筆會由 0 開始計算，範例如下：

```js
let data = ['red', 'yellow', 'green'];
console.log( data );
// 輸出結果 ----
(3) ['red', 'yellow', 'green']
0: "red"
1: "yellow"
2: "green"
length: 3
[[Prototype]]: Array(0)
// ----
```

從上述範例結果中，可以得知該陣列的每筆資料內容、長度以及資料結構類型，此時假設如果要取得該陣列的第二筆資料內容時，可以在該變數後方加上 `[1]`，如下所示：

```js
let data = ['red', 'yellow', 'green'];
console.log( data[1] ); // 第一筆以 [0] 表示，因此第二筆為 [1]，依此類推
// 輸出結果為 yellow
```

#### 陣列取值賦予新變數

可指定陣列中的資料，並將該資料內容賦予至一個新變數中，範例如下：

```js
let musicGenres = ['Pop', 'Rock', 'Rap'];
let myHobby = musicGenres[0];
console.log( myHobby );
// 輸出結果為 Pop
```

#### 取得陣列長度

先前在型別有提到可使用 `length` 來查詢變數的內容長度，而該作法在陣列上的使用方式也是相同的，範例如下：

```js
let ary = [1, 2, 3, 4, 5];
let aryLength = ary.length;
console.log( aryLength );
// 輸出結果為 5
```

#### 新增陣列資料

以空陣列為例，如下所示：

```js
// 範例一：指定陣列中第一筆資料並賦予值
let ary = [];
ary[0] = '新增資料';
console.log( ary );
// 輸出結果 ----
['新增資料']
0: "新增資料"
length: 1
[[Prototype]]: Array(0)
// ----
```

```js
// 範例二：分別指定陣列中第一筆資料與第三筆資料並賦予值
let ary = [];
ary[0] = '第一筆資料';
ary[2] = '第二筆資料';
console.log( ary, ary[1] );
// 輸出結果 ----
(3) ['第一筆資料', empty, '第二筆資料'] undefined
0: "第一筆資料"
2: "第二筆資料"
length: 3
[[Prototype]]: Array(0)
// ----
```

> 範例二中的第二筆資料因為沒有賦予值，因此該筆資料為空資料 `empty`，但是空值不代表該筆資料不存在，而是尚未定義資料內容（undefined），因此也會被納入陣列的長度（length）中。

#### push 新增資料

`push()` 是新增資料到陣列中的一種方法，使用該方式所加入的值，會被放到陣列**最末端**的位置，範例如下：

```js
let colors = ['red', 'yellow', 'green'];
colors.push('blue');
console.log( colors );
// 輸出結果 ----
(4) ['red', 'yellow', 'green', 'blue'] // blue 位置在最後
0: "red"
1: "yellow"
2: "green"
3: "blue"
length: 4
[[Prototype]]: Array(0)
// ----
```

#### unshift 新增資料

運作的原理與 `push()` 相同，差異在於透過 `unshift()` 所加入陣列的值，位置會在該陣列的**最前端**，範例如下：

```js
let colors = ['red', 'yellow', 'green'];
colors.unshift('blue');
console.log( colors );
// 輸出結果 ----
(4) ['blue', 'red', 'yellow', 'green'] // blue 位置在最前
0: "blue"
1: "red"
2: "yellow"
3: "green"
length: 4
[[Prototype]]: Array(0)
// ----
```

#### pop 刪除資料

在一個陣列後方加入 `pop()` 方法之後，該陣列的**最後一筆**資料會被刪除，範例如下：

```js
let colors = ['red', 'yellow', 'green'];
colors.pop();
console.log( colors );
// 輸出結果 ----
(2) ['red', 'yellow'] // 最後一筆 green 被刪除
0: "red"
1: "yellow"
length: 2
[[Prototype]]: Array(0)
// ----
```

#### shift 刪除資料

運作的原理與 `pop()` 相同，差異在於 `shift()` 所刪除的資料，是陣列中的**第一筆**，範例如下：

```js
let colors = ['red', 'yellow', 'green'];
colors.shift();
console.log( colors );
// 輸出結果 ----
(2) ['yellow', 'green'] // 第一筆 red 被刪除
0: "yellow"
1: "green"
length: 2
[[Prototype]]: Array(0)
// ----
```

#### splice 刪除指定資料

使用 `splice()` 方法刪除陣列中的資料時，會加入兩個參數，範例如下：

```js
let colors = ['red', 'yellow', 'green', 'blue'];
colors.splice(1, 2);
console.log( colors );
// 輸出結果 ----
(2) ['red', 'blue'] // 從 green 開始，刪除兩筆資料
0: "red"
1: "blue"
length: 2
[[Prototype]]: Array(0)
// ----
```

上述範例中，`splice(1, 2)` 兩個參數依序分別代表刪除資料的起始位置，以及刪除資料的筆數，因此會以第二筆開始刪除，並刪除兩筆資料，因此 `green` 與 `blue` 被刪除。

> 無論使用上述任何方法新增、刪除陣列中的資料，該陣列的長度都會有所改變。

> 關於陣列的處理方法還有 `filter()` , `find()` , `forEach()` , `map()` , `every()` , `some()` , `reduce()`，以上內容會在函式（function）的部分說明。

### 物件（Object）

當一筆資料需要詳細紀錄不同的細部資訊時，會以物件的形式來表示，並賦予至一個變數中，範例如下：

```js
let myInfo = {
    name: 'Mark', // 屬性: 屬性值
    sex: 'male',
    age: 18,
    email: 'test@email.com',
    isSingle: true
};
console.log( myInfo );
// 輸出結果 ----
{name: 'Mark', sex: 'male', age: 18, email: 'test@email.com', isSingle: true}
age: 18
email: "test@email.com"
isSingle: true
name: "Mark"
sex: "male"
[[Prototype]]: Object
// ----
```

> 物件的內容會放入一個大括號內，而物件中每個細項會有一個屬性名稱與對應的屬性值，且屬性之間會使用半形逗號隔開。

#### 取得物件資料

物件中會存在各種不同名稱的屬性與對應的值，如果要取得指定的屬性值，第一種方式會使用 `.` 符號，範例如下：

```js
let myInfo = {
    name: 'Mark',
    age: 18,
    isSingle: true
};
console.log( myInfo.name ); // 取得物件 myInfo 中的屬性 name 的值
// 輸出結果為 Mark
```

除了前面提到使用 `.` 來取得屬性值，還可以使用中括號 `[屬性名稱]` 的方式來取得，範例如下：

```js
let myInfo = {
    name: 'Mark',
    age: 18,
    isSingle: true
};
console.log( myInfo['name'] ); // 取得物件 myInfo 中的屬性 name 的值
// 輸出結果為 Mark
```

此外，也可以將物件的屬性名稱賦予至一變數，並透過 `[變數]` 的方式來取得物件中的屬性值，範例如下：

```js
let myInfo = {
    name: 'Mark',
    age: 18,
    isSingle: true
};
let myName = 'name';
console.log( myInfo[myName] ); // 原理等同於 myInfo['name']
// 輸出結果為 Mark
```

> 使用 `.` 或是 `[]` 都可以取得物件中的屬性值，而前者在取得某些 JSON 格式的資料時可能會導致程式碼無法辨識。

#### 新增物件屬性

以空物件為例，如下所示：

```js
let myInfo = {};
myInfo.name = 'Mark'; // 在 myInfo 物件中新增一個 name 屬性，並賦予屬性值 'Mark'
console.log( myInfo );
```

#### 修改物件屬性值

物件中的屬性值修改方式原理與變數相同，範例如下：

```js
let myInfo = {
    name: 'Mark',
    age: 18,
    isSingle: true
};
// 以下透過賦值運算子改變屬性值
myInfo.name = 'Fuck';
myInfo.age += 1;
isSingle = false;
console.log( myInfo );
// 輸出結果 ----
{name: 'Fuck', age: 19, isSingle: true}
age: 19
isSingle: true
name: "Fuck"
[[Prototype]]: Object
// ----
```

#### 刪除物件資料

物件中的屬性，可以透過 `delete` 來指定刪除，範例如下：

```js
let myInfo = {
    name: 'Mark',
    age: 18,
    isSingle: true
};
delete myInfo.isSingle; // 刪除 myInfo 物件中的 isSingle 屬性與值
console.log( myInfo, myInfo.isSingle );
// 輸出結果 ----
{name: 'Mark', age: 18} undefined // 刪除後找不到 isSingle 相關屬性
age: 18
name: "Mark"
[[Prototype]]: Object
// ----
```
