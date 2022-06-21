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

> 宣告變數的名稱可使用駝峰式命名，此外，若撞到部分保留字，會無法宣告，保留字可參考此[連結](http://www.w3bai.com/zh-TW/js/js_reserved.html)。

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

型別為字串的值會加上單或雙引號來表示。

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

---

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

---

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
// (3) ['red', 'yellow', 'green']
// 0: "red"
// 1: "yellow"
// 2: "green"
// length: 3
// [[Prototype]]: Array(0)
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
// ['新增資料']
// 0: "新增資料"
// length: 1
// [[Prototype]]: Array(0)
// ----
```

```js
// 範例二：分別指定陣列中第一筆資料與第三筆資料並賦予值
let ary = [];
ary[0] = '第一筆資料';
ary[2] = '第二筆資料';
console.log( ary, ary[1] );
// 輸出結果 ----
// (3) ['第一筆資料', empty, '第二筆資料'] undefined
// 0: "第一筆資料"
// 2: "第二筆資料"
// length: 3
// [[Prototype]]: Array(0)
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
// (4) ['red', 'yellow', 'green', 'blue'] // blue 位置在最後
// 0: "red"
// 1: "yellow"
// 2: "green"
// 3: "blue"
// length: 4
// [[Prototype]]: Array(0)
// ----
```

#### unshift 新增資料

運作的原理與 `push()` 相同，差異在於透過 `unshift()` 所加入陣列的值，位置會在該陣列的**最前端**，範例如下：

```js
let colors = ['red', 'yellow', 'green'];
colors.unshift('blue');
console.log( colors );
// 輸出結果 ----
// (4) ['blue', 'red', 'yellow', 'green'] // blue 位置在最前
// 0: "blue"
// 1: "red"
// 2: "yellow"
// 3: "green"
// length: 4
// [[Prototype]]: Array(0)
// ----
```

#### pop 刪除資料

在一個陣列後方加入 `pop()` 方法之後，該陣列的**最後一筆**資料會被刪除，範例如下：

```js
let colors = ['red', 'yellow', 'green'];
colors.pop();
console.log( colors );
// 輸出結果 ----
// (2) ['red', 'yellow'] // 最後一筆 green 被刪除
// 0: "red"
// 1: "yellow"
// length: 2
// [[Prototype]]: Array(0)
// ----
```

#### shift 刪除資料

運作的原理與 `pop()` 相同，差異在於 `shift()` 所刪除的資料，是陣列中的**第一筆**，範例如下：

```js
let colors = ['red', 'yellow', 'green'];
colors.shift();
console.log( colors );
// 輸出結果 ----
// (2) ['yellow', 'green'] // 第一筆 red 被刪除
// 0: "yellow"
// 1: "green"
// length: 2
// [[Prototype]]: Array(0)
// ----
```

#### splice 刪除指定資料

使用 `splice()` 方法刪除陣列中的資料時，會加入兩個參數，範例如下：

```js
let colors = ['red', 'yellow', 'green', 'blue'];
colors.splice(1, 2);
console.log( colors );
// 輸出結果 ----
// (2) ['red', 'blue'] // 從 green 開始，刪除兩筆資料
// 0: "red"
// 1: "blue"
// length: 2
// [[Prototype]]: Array(0)
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
// {name: 'Mark', sex: 'male', age: 18, email: 'test@email.com', isSingle: true}
// age: 18
// email: "test@email.com"
// isSingle: true
// name: "Mark"
// sex: "male"
// [[Prototype]]: Object
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
// {name: 'Fuck', age: 19, isSingle: true}
// age: 19
// isSingle: true
// name: "Fuck"
// [[Prototype]]: Object
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
// {name: 'Mark', age: 18} undefined // 刪除後找不到 isSingle 相關屬性
// age: 18
// name: "Mark"
// [[Prototype]]: Object
// ----
```

---

## 物件結合陣列

陣列除了可以放入一般的變數之外，也能夠放入多個物件，範例如下：

```js
// 範例：水果的詳細資訊
let fruitDetail = [
    {
        name: 'Apple',
        price: 30,
        isSoldOut: false
    },{
        name: 'banana',
        price: 20,
        isSoldOut: true
    }
]
console.log( fruitDetail );
// 輸出結果 ----
// (2) [{…}, {…}]
// 0: {name: 'Apple', price: 30, isSoldOut: false}
// 1: {name: 'banana', price: 20, isSoldOut: true}
// length: 2
// [[Prototype]]: Array(0)
// ----
```

> 陣列中的每個物件之間會使用一個半形逗號隔開。

先前提到陣列會使用 `[順序]` 的方式來取值，而在陣列內容結構為物件的情況下也是相同的，會使用 `[]` 來指定物件，並透過 `.` 或是 `[]` 來取得屬性內容，範例如下：

```js
let fruitDetail = [
    { // 第 1 筆 [0]
        name: 'Apple',
        price: 30,
        isSoldOut: false
    },{ // 第 2 筆 [1]
        name: 'banana',
        price: 20,
        isSoldOut: true
    }
];
console.log( fruitDetail[1] ); // 取得陣列 fruitDetail 的第二個物件內容
console.log( fruitDetail[1].name ); // 指定陣列 fruitDetail 的第二個物件，並取得該物件中屬性 name 的值
// 輸出結果1 ----
// {name: 'banana', price: 20, isSoldOut: true}
// isSoldOut: true
// name: "banana"
// price: 20
// [[Prototype]]: Object
// 輸出結果2 ----
// banana
// ----
```

### JSON 格式

JSON 是用於程式語言的一種資料結構，方便閱讀，目前也是多種語言通用的資料格式，範例如下：

```js
// 範例 - 來源取自 wikipedia
[
    {
        "text": "This is the text",
        "color": "dark_red",
        "bold": "true",
        "strikethough": "true",
        "clickEvent":
        {
            "action": "open_url",
            "value": "zh.wikipedia.org"
        },
        "hoverEvent":
        {
            "action": "show_text",
            "value":
            {
                "extra": "something"
            }
        }
    },
    {
        "translate": "item.dirt.name",
        "color": "blue",
        "italic": "true"
    }
];
```

> 若是使用 Chrome 瀏覽器，可安裝 JSONView 擴充功能，該工具可將網頁上壓縮後的 JSON 格式資料進行自動整理以提高可讀性。

### 物件巢狀運用

物件中的屬性值也能以物件結構表示，範例如下：

```js
// 巢狀物件
let fruit = { // 外層物件
    name: 'Apple',
    price: 30,
    isSoldOut: { // 內層物件
        storeA: true,
        storeB: false
    }
}
console.log( fruit ); // 取得物件整體內容
console.log( fruit.isSoldOut.storeA ); // 取得物件內的物件資料
// 輸出結果1----
// {name: 'Apple', price: 30, isSoldOut: {…}}
// isSoldOut: {storeA: true, storeB: false}
// name: "Apple"
// price: 30
// [[Prototype]]: Object
// 輸出結果2----
// true
// ----
```

#### 取得 JSON 資料

**範例一**

以此[公開資料](https://api.kcg.gov.tw/api/service/get/4278fc6a-c3ea-4192-8ce0-40f00cdb40dd)為例，假設已將該資料賦予至變數 `jsonData` 中，嘗試取得屬性 `data` 中第 3 筆資料的 `車站中文名稱` 屬性值，做法如下：

```js
console.log( jsonData.data[2].車站中文名稱);
// 輸出結果為 草衙
```

**範例二**

```js
let data = {
  "contentType": "application/json; charset=utf-8",
  "isImage": false,
  "data": {
    "XML_Head": {
      "Listname": "1",
      "Language": "C",
      "Orgname": "397000000A",
      "Updatetime": "2021/01/20 08:40:00",
      "Infos": {
        "Info": [
          {
            "Id": "C1_397000000A_000230",
            "Status": "2",
            "Name": "田寮月世界",
            "Zone": "",
            "Toldescribe": "田寮「月世界」特殊景觀在地理學上稱為「惡地」，是由於地殼的「回春作用」，經年累月的經由雨水與河水強烈侵蝕，將泥沙堆積在泥岩上，地層變動後，泥沙更與泥岩混合再經由風化、沉積作用，形成今日地貌，僅適於耐旱、耐鹽的淺根植物（如：箭竹）、濱海植物生長。從田寮到旗山台28線沿路除了月世界景觀，還有大小不等的二十多個泥火山，常呈現間歇性的噴發現象，噴發的規模則視地底天然氣與泥漿的累積壓力而定，噴發後的泥流堆積地區。",
            "Description": "田寮「月世界」特殊景觀在地理學稱為「惡地」，經年累月由雨、河水侵蝕，將泥沙堆積在泥岩上，泥沙與泥岩混合經由風化形成。",
            "Tel": "886-7-6367036",
            "Add": "高雄市823田寮區崇德里月球路36號",
            "Zipcode": "823",
            "Travellinginfo": "無障礙交通：高鐵台南站 → 沙旗美月世界快線公車 → 月世界 → 旗山高鐵左營站 → 旗美快線公車 → 旗山 → 轉搭沙旗美月世界快線公車 → 月世界",
            "Opentime": "遊客中心：09:00–17:00月世界：全天候開放",
            "Gov": "397000000A",
            "Px": "120.38898",
            "Py": "22.88600"
          },
          {
            "Id": "C1_397000000A_000234",
            "Status": "2",
            "Name": "西子灣風景區",
            "Zone": "",
            "Toldescribe": "西子灣以夕陽美景及天然礁石聞名，區內包括了西子灣海水浴場、海濱公園、打狗英國領事館....等景點；可觀海景、遠眺高雄港；海水浴場極富熱帶氣息、南國風情，每當夜幕低垂，晚霞的照耀，漁船燈火閃爍其間，呈現海天一色美景。",
            "Description": "西子灣以夕陽美景及天然礁石聞名，區內包括了西子灣海水浴場、海濱公園、打狗英國領事館....等景點。",
            "Tel": "886-7-5250005",
            "Add": "高雄市804鼓山區蓮海路51號",
            "Zipcode": "804",
            "Travellinginfo": "搭高鐵至左營站下或搭臺鐵至高雄站下 → 轉搭高雄捷運至西子灣站下 → 轉搭高雄市公車(99路、橘1A路)至西子灣站下。",
            "Opentime": "西子灣海水浴場：10:00–16:00",
            "Gov": "397000000A",
            "Px": "120.26391",
            "Py": "22.62442"
          }
        ]
      }
    }
  },
  "id": "b69ffff9-23a5-44a6-a398-089b11a5f84c",
  "success": true
};
```

上述內容為一個 JSON 格式資料，嘗試完成註解中的內容：

```js
// 取得 Info 的陣列資料，並賦予至 newData 變數
let newData = data.data.XML_Head.Infos.Info;

// 取得 Info 陣列中第 2 筆資料的 Opentime 屬性值
console.log( data.data.XML_Head.Infos.Info[1].Opentime ); // 做法一
console.log( newData[1].Opentime ); // 做法二
// 輸出結果皆為 西子灣海水浴場：10:00–16:00
```

### 物件結合 if 判斷

簡單範例如下：

```js
let myData = {
    name: 'Mark',
    age: 18,
    state: '',
}
if( myData.age >= 18 ){
    myData.state = '符合入場條件';
}else{
    myData.state = '不符合入場條件';
}
console.log( myData );
// 輸出結果 ----
// {name: 'Mark', age: 18, state: '符合入場條件'}
// age: 18
// name: "Mark"
// state: "符合入場條件"
// [[Prototype]]: Object
// ----
```

### 物件結合陣列與 if 判斷

簡單範例如下：

```js
// 情境：判斷 Mark 是否成年，若未成年則不得入場
let peopleData = [
    {
        name: 'Mark',
        age: 18,
        state: ''
    },{
        name: 'Vivian',
        age: 15,
        state: ''
    }
];
if( peopleData[0].age >= 18 ){
    peopleData[0].state = '符合入場條件'; // 符合條件就重新賦予 state 屬性值
}else{
    peopleData[0].state = '不符合入場條件';
}
console.log( peopleData[0] );
// 輸出結果 ----
// {name: 'Mark', age: 18, state: '符合入場條件'}
// age: 18
// name: "Mark"
// state: "符合入場條件"
// [[Prototype]]: Object
// ----
```

> 以上只是運作流程與邏輯的參考，而通常資料會有數筆，因此如果像上述範例一樣每有一筆資料就處理一次，效率就會很差且不易閱讀，因此通常會透過迴圈的方式來處理大批資料。

---

## 函式

函式（function）的作用主要是把一系列相關的程式透過一個指令來包裝，並視情況執行，起手式如下：

```js
function showText(){
    console.log('一段文字內容');
}
showText(); // 執行函式 showText
// 輸出結果為 一段文字內容
```

> 一組函式在註冊完成後並不會立即執行，需要透過 `functionName();` 方式來執行，而該函式在執行完內容後，就會終止。

### 巢狀函式

範例如下：

```js
function showText(){
    console.log('文字內容一');
    showText2();
}
function showText2(){
    console.log('文字內容二');
}
showText();
// 輸出結果 ----
// 文字內容一
// 文字內容二
// ----
```

> 函式中若存在別的函式，則會先搜尋是否有該函式的存在，若有就執行該函式的內容，執行完畢後再跳回原本的函式接續執行後續的內容。

### 代入參數

一個函式在執行時，是可以帶入參數的，如下所示：

```js
function calculate(num, num2){
    console.log( num + num2 );
}
calculate(2, 3);
console.log( num, num2 );
// 輸出結果 ----
// 5
// num is not defined
// ----
```

上述範例在執行函式 `calculate` 時，代入了 `2`、`3` 兩個參數，此時第 2 行開始運算並得出結果為 `5`，而在函式外嘗試取得 `num` 的值後，輸出結果為顯示找不到相關內容，由此可知函式所代入的參數只能在該函式中作運用。

### return 回傳結果

前面提到函式所代入的參數只能使用在該函式中，但如果要在函式外做使用，可以透過 `return` 來將**函式運算後的結果**回傳到指定的地點，範例如下：

```js
function calculate(num, num2){
    return num + num2; // 將運算結果回傳到第 4 行
}
let result = calculate(5, 10);
console.log( result );
// 輸出結果為 15
```

除了上述範例在函式內直接回傳 `num + num2` 的運算結果之外，也可以先將運算結果賦予到一變數中，再透過 `return` 回傳**該變數的值**，如下所示：

```js
function calculate(num, num2){
    let sum = num + num2;
    return sum;
}
let result = calculate(5, 10);
console.log( `運算結果等於${result}` );
// 輸出結果為 '運算結果等於15'
```

> 函式內所宣告的變數與參數一樣，只能在該函式內使用，而 `return` 回傳的只有運算後的結果，並非該變數本身。

### return 中斷函式

return 除了能夠回傳結果，還具有中斷函式執行的作用，範例如下：

```js
function calculate(num, num2){
    let sum = num + num2;
    return sum;  // 回傳結果後在此中斷
    console.log('一段文字內容'); // 因 return 中斷而不執行
}
let result = calculate(5, 10);
console.log( result );
// 輸出結果為 15
```

### return 使用情境

**情境一**

```js
// 判斷成績是否及格
function checkScore(score){
  if( 100 >= score && score >= 60 ){
      return '成績及格';
  }else if( 60 > score && score >= 0 ){
      return '成績不及格'; // 回傳結果至 exam 後程式在此中斷
  }
  return '資料有誤'; // 回傳結果至 exam2 後程式在此中斷
}
let exam = checkScore(50);
let exam2 = checkScore(120);
console.log( exam, exam2 );
// 輸出結果為 '成績不及格' '資料有誤'
```

**情境二**

```js
// 判斷兩數字相除是否可整除
function calculate(num, num2){
  let remainder = num % num2;
  if( remainder == 0 ){
      return '可以整除';
  }else{
      return `餘數為${remainder}，不可整除`; // 回傳結果至 result 後程式在此中斷
  }
}
let result =  calculate(100, 12);
console.log( result );
// 輸出結果為 '餘數為4，不可整除'
```

**情境三**

```js
// 判斷計算結果總次數
let calcNum = 0; // 全域變數
function calculate(num, num2){
  calcNum += 1;
  let remainder = num % num2;
  if( remainder == 0 ){
      return '可以整除';
  }else{
      return `餘數為${remainder}，不可整除`; // 回傳結果至 result 後程式在此中斷
  }
}
let result =  calculate(100, 50);
console.log( result, `目前總共計算${calcNum}次` );
// 輸出結果為 '可以整除' '目前總共計算1次'
```

> 從範例中第 4 行可得知，當函式中找不到宣告的變數時，會往全域搜尋是否存在相同名稱的變數。

---

## DOM

DOM（Document Object Model）簡單來說，就是將一個 HTML 的文件組成內容（標籤、文字、圖片等），以樹狀結構來表示的模型，詳細資料可參考此[連結](https://developer.mozilla.org/zh-TW/docs/Web/API/Document_Object_Model)。

### querySelector

透過 querySelector 可以選取網頁中的元素（標籤、class 選擇器、id 選擇器），語法格式為 `document.querySelector('網頁元素')`，以 id 選擇器為例，如下所示：

```html
<!-- HTML -->
<botton id="btn">Botton</botton>
```

```js
// js
const el = document.querySelector('#btn');
console.log(el);
// 輸出結果 ----
// <botton id="btn">Botton</botton>
// ----
```

### querySelectorAll

雖然 querySelector 可以選取網頁中的元素，但是相同名稱的元素存在多個時，只有第一個會被選取到，此時可以使用 querySelectorAll 來選取多個元素，範例如下：

```html
<!-- HTML -->
<ul>
  <li><a href="#">Link1</a></li>
  <li><a href="#">Link2</a></li>
  <li><a href="#">Link3</a></li>
</ul>
```

```js
// js
const els = document.querySelectorAll('a');
console.log(a);
// 輸出結果 ----
// NodeList[3]
// 0: a
// 1: a
// 2: a
// length: 3
// [[Prototype]]: NodeList
// ----
```

上述 JS 範例中得知，透過 querySelectorAll 的方式選取到所有的 a 標籤，而資料結構 `NodeList` 是一種有序的節點列表，也屬於陣列，因此可以透過中括號的方式來指定想要選取的內容，延續先前範例，如下所示：

```js
const els = document.querySelectorAll('a');
console.log(els[2]);
// 輸出結果為 a
```

> querySelector 會回傳一個 DOM，而 querySelector 則是回傳一個陣列。

### textContent

如果需要修改元素的純文字內容，可以使用 textContent，範例如下：

```html
<!-- HTML -->
<h1 class="title">Title</h1>
```

```js
// js
const el = document.querySelector('.title');
el.textContent = 'hello';
```

上述 JS 範例中，第 3 行將元素 `.title` 的文字內容 Title 修改為 Hello。

### innerHTML

與先前 textContent 的差異在於，innerHTML 除了純文字以外，還可以新增 HTML 標籤與相關內容，範例如下：

```html
<!-- HTML -->
<div class="links"></div>
```

```js
// js
const el = document.querySelector('.links');
el.innerHTML = `
<ul><li>Google</li>
<li>Yahoo</li>
<li>Youtube</li></ul>`;
```

上述 JS 範例中，第 3 行使用 `innerHTML` 新增一組列表 `ul` 與其文字內容，而所新增的元素內容可透過**反引號**（樣板字面值）的方式來包覆。

> 使用 `innerHTML` 新增標籤後，若區塊內原先已有其他標籤，舊有的標籤內容會直接被清空並取代為新增的標籤內容。

此外，透過 innerHTML 所加入的標籤中，也能夠帶入變數，延續先前範例，如下所示：

```js
// 未帶入變數
const el = document.querySelector('.links');
el.innerHTML = `
<ul><li>Google</li>
<li>Yahoo</li>
<li><a href="https://www.youtube.com/">Youtube</a></li></ul>`;
```

```js
// 帶入變數
const el = document.querySelector('.links');
const myLink = 'https://www.youtube.com/';
el.innerHTML = `
<ul><li>Google</li>
<li>Yahoo</li>
<li><a href=${myLink}>Youtube</a></li></ul>`;
```

### setAttribute

透過 setAttribute 可設定 HTML 標籤中的屬性，格式為 `setAttribute('標籤屬性','屬性內容')`，範例如下：

```html
<!-- HTML -->
<a href="#" class="link">Link</a>
```

```css
/* css */
.text-primary {
  color: red;
}
```

```js
// js
const myLink = document.querySelector('.link');
myLink.setAttribute('href','https://www.google.com/');
myLink.setAttribute('class','red');
```



### 取得節點內容

前面介紹到的 textContent、innerHTML、setAttribute 都是屬於寫入內容，若要取得內容可以使用以下做法：

**取得標籤中的純文字**

textContent 可寫入純文字，也可以取得標籤中的純文字內容，範例如下：

```html
<!-- HTML -->
<h1 class="title">Title</h1>
```

```js
// js
const el = document.querySelector('.title');
console.log(el.textContent); // 取得 h1 標籤中的純文字內容
// 輸出結果為 Title
```

> 透過 textContent 所寫入的純文字，也可透過上述方式來取得純文字內容。

**取得 HTML 標籤**

innerHTML 可寫入標籤，同時也可以取得標籤，範例如下：

```html
<!-- HTML -->
<div class="links">
  <ul>
    <li>Google</li>
    <li>Yahoo</li>
    <li>Youtube</li>
  </ul>
</div>
```

```js
// js
const el = document.querySelector('.links');
console.log(el.innerHTML); // 取得 .links 區塊中的標籤內容
// 輸出結果 ----
// <ul>
//   <li>Google</li>
//   <li>Yahoo</li>
//   <li>Youtube</li>
// </ul>
// ----
```

> 透過 innerHTML 所寫入的標籤，也可透過上述方式來取得標籤內容。

**取得標籤屬性內容**

標籤屬性相關內容可透過 getAttribute 來取得，範例如下：

```html
<!-- HTML -->
<a href="https://www.google.com/" class="link">Link</a>
```

```js
// js
const el = document.querySelector('.link');
console.log(el.getAttribute('href')); // 取得 a 標籤的 href 屬性內容
console.log(el.getAttribute('class')); // 取得 a 標籤的 class 屬性名稱
// 輸出結果 ----
// 'https://www.google.com/'
// 'link'
// ----
```

### 表單值取得與修改

```html
<!-- HTML -->
<input type="text" class="txt" value="文字內容">
<select name="" id="city">
  <option value="高雄">高雄</option>
  <option value="台北">台北</option>
</select>
```

**取得表單值**

```js
// js
const el = document.querySelector('.txt');
const el2 = document.querySelector('#city');
console.log(el.value, el2.value);
// 輸出結果為 '文字內容' '高雄'
```

**修改表單值**

```js
// js
const el = document.querySelector('.txt');
el.value = '修改後的文字內容'; // 重新賦予 .txt 的 value 值
const el2 = document.querySelector('#city');
el2.value = '台北'; // 變更預設顯示的值
```

### nodeName

透過 nodeName 可以回傳目前 DOM 的節點名稱，範例如下：

```html
<!-- HTML -->
<button type="button" class="btn">Button</button>
```

```js
// js
const btn = document.querySelector('.btn');
console.log(btn.nodeName);
// 輸出結果為 BUTTON
```

---

## event 事件

event 表示在一個 DOM 元素上所觸發的事件，像常見的滑鼠點擊就屬於事件的一種，其他事件可參考此[連結](https://www.w3school.com.cn/jsref/dom_obj_event.asp)。

### addEventListener

DOM 的觸發事件可以透過 addEventListener() 方法來進行註冊，而該方法會有三個參數，分別是事件名稱、觸發後執行的函式、捕獲或冒泡階段的執行（在此不做說明），以點擊事件 `click` 為例，範例如下：

```html
<!-- HTML -->
<button type="button" class="btn">Button</button>
```

```js
// js
const btn = document.querySelector('.btn');
btn.addEventListener('click', function(e) { // 註冊事件監聽
  console.log('已被點擊');
})
// 觸發事件時輸出結果為 已被點擊
```

> 事件監聽中的函式僅在事件觸發後才會執行。

事件監聽中的函式，會帶入一個參數 `e`（event），而這個參數的結構是一個物件，主要是存放與該事件有關的所有屬性與其相關資訊，以前面範例來說，當事件觸發時，回傳結果如下：

```js
PointerEvent {isTrusted: true, pointerId: 0, width: 1, height: 1, pressure: 0, …}
```

因為是物件的關係，也可以指定想要顯示的資訊，如事件觸發後，回傳觸發事件的 DOM 當前位置，做法如下：

```js
// js
const btn = document.querySelector('.btn');
btn.addEventListener('click', function(e) {
  console.log(e.target);
})
// 觸發事件時輸出結果為 <button type="button" class="btn">Button</button>
```

### target 當前位置

在上個範例有提到，事件監聽中的函式所帶入的參數 `e` 會回傳一個紀錄所有相關屬性的物件，而其中的屬性 `target` 代表觸發事件的元素位置。

```html
<!-- HTML -->
<button type="button" class="btn">Button</button>
```

```js
// js
const btn = document.querySelector('.btn');
btn.addEventListener('click', function(e) {
  console.log(e.target);
})
// 觸發事件時輸出結果為 <button type="button" class="btn">Button</button>
```

但是有些時候可能會希望一個範圍內的所有元素，都能夠觸發事件，此時事件監聽的對象也會視情況而有所不同，範例如下：

```html
<!-- HTML -->
<ul class="list">
  <li class="item-1">item-1</li>
  <li class="item-2">item-2</li>
  <li class="item-3">item-3</li>
  <li class="item-4">item-4</li>
</ul>
```

```js
// js
const els = document.querySelector('.list'); // 監聽整個 ul 範圍
els.addEventListener('click', function(e){
  console.log(e.target); // 輸出觸發對象的純文字內容
});
// 觸發事件時輸出結果 ----
// 點擊 item-1 範圍時，輸出結果為 <li class="item-1">item-1</li>
// 點擊 item-2 範圍時，輸出結果為 <li class="item-2">item-2</li>
// 點擊 item-3 範圍時，輸出結果為 <li class="item-3">item-3</li>
// 點擊 item-4 範圍時，輸出結果為 <li class="item-4">item-4</li>
// ----
```

從上方範例可得知，因為事件監聽的範圍為整個 `ul`，因此當範圍內的不同元素所佔有的範圍被點擊時，輸出的結果也會對應到不同的內容。

**簡單範例應用**

```html
<!-- HTML -->
<ul class="list">
  <li>
    <h1 class="title">Title</h1>
  </li>
  <li>
    <input type="button" class="btn" value="Button">
  </li>
</ul>
```

```js
// js
const list = document.querySelector('.list');
list.addEventListener('click', function(e) {
  if(e.target.nodeName == 'INPUT') {
    console.log('點擊到按鈕');
  }else{
    console.log('未點擊到按鈕', `目前點擊的對象是 ${e.target.nodeName}`);
  }
})
```

如上述範例，事件監聽範圍為 `.list` 區塊，當範圍內點擊事件觸發時，判斷觸發對象的節點名稱是否為 `INPUT` 而輸出對應的內容。

### 取消默認行為

HTML 標籤會存在一些默認行為，以 a 標籤來說，點擊標籤的連結會跳轉到指定的頁面就屬於一種默認的行為，如果要避免這些行為的話，可以使用 `e.preventDefault()` 方法來達成，範例如下：

```html
<!-- HTML -->
<a href="https://www.google.com/">Google</a>
```

```js
// js
const link = document.querySelector('a');
link.addEventListener('click', function(e) {
  e.preventDefault();
  console.log('未跳轉新頁面');
})
// 當點擊 a 標籤時，網頁不會跳轉，且輸出結果為 '未跳轉新頁面'
```



---

## 迴圈

相同性質的資料若資料筆數過多，通常會透過迴圈的方式重複執行相同的事，來取得資料內容。

### for 迴圈

```js
// 範例：運作原理
for( var i=0; i<3; i++ ){
    console.log( i );
}
// 輸出結果 ----
// 0
// 1
// 2
// ----
```

上述範例中，`for` 小括號的三個項目依序分別表示**初始狀態**、**執行條件**、**變更值**，在此宣告一個變數 `i` 初始值為 `0`，當 `i` 值小於 `3` 的判斷結果為 `true` 時，執行大括號中的內容，每執行完一次 `i` 值 `+1`，接著運行第二次直到不滿足執行條件為止。

> for 迴圈的小括號中使用 `var` 宣告變數 `i` 時，該變數會屬於全域變數。

#### for 結合陣列

**情境一**

```js
// 情境：列出所有種類的水果名稱
let fruitDetail = [
    {
        name: 'Apple',
        price: 30,
    },{
        name: 'banana',
        price: 20,
    }
]
let fruitNum = fruitDetail.length;
for( var i=0; i<fruitNum; i++ ){
    console.log( fruitDetail[i].name );
}
// 輸出結果 ----
// Apple
// banana
// ----
```

**情境二**

```js
// 情境：加總所有學校的學生人數
let school = [
    {
        name: '學校A',
        studentNum: 35
    },{
        name: '學校B',
        studentNum: 32
    }
];
let schoolNum = school.length;
let studentTotal = 0;
for( let i=0; i<schoolNum; i++ ){
    studentTotal += school[i].studentNum;
}
console.log( `全部學生總共有${studentTotal}人` );
// 輸出結果為 全部學生總共有67人
```

#### for 結合 if

```js
// 範例：列出正在下雨的城市
let cityStatus = [
    {
        city: '高雄',
        state: '晴天'
    },{
        city: '台南',
        state: '下雨'
    },{
        city: '台北',
        state: '下雨'
    }
];
let cityNum = cityStatus.length;
for( let i=0; i<cityNum; i++ ){
    if( cityStatus[i].state == '下雨' ){
        console.log( `${cityStatus[i].city}天氣為雨天` );
    }
}
// 輸出結果 ----
// 台南天氣為雨天
// 台北天氣為雨天
// ----
```

#### break 中斷迴圈

若希望 for 迴圈在執行過程中當滿足了某些條件後，就終止迴圈執行，可以使用 `break` 來中斷動作，範例如下：

```js
// 範例：集點活動，找出最先累積滿 100 點的人
let people = [
    {
        name: 'Mark',
        points: 89
    },{
        name: 'Vivian',
        points: 102
    },{
        name: 'Leo',
        points: 115
    }
];
for( let i=0; i<people.length; i++ ){
    if( people[i].points >= 100 ){
        console.log( `最先累積滿100點的人是${people[i].name}，總共有${people[i].points}點!` );
        break; // 迴圈運行到第2筆時達成條件，因此終止迴圈
    }
}
// 輸出結果為 最先累積滿100點的人是Vivian，總共有102點!
```

> 範例中若未加上 `break`，則輸出結果會列出所有滿足 `points >= 100` 的內容，而 break 僅能在 for 迴圈中使用。

### forEach 迴圈

```js
// 範例：運作原理
let data = ['red','green','blue'];
data.forEach(function(item, index, array) {
  console.log(item, index, array);
});
console.log('結束迴圈');
// 輸出結果 ----
// red 1 (3) ['blue', 'red', 'green']
// green 2 (3) ['blue', 'red', 'green']
// blue 0 (3) ['blue', 'red', 'green']
// 結束迴圈
// ----
```

上述範例中，第 3 行的部分可以看到 `forEach()` 會放入一個函式，而這個函式的執行次數，會根據陣列中的資料筆數而定，以範例來說，`data` 陣列中的資料總共有三筆，因此會執行三次，接著該函式可以帶入三個變數，分別表示**當前對象的值**、**索引值**、**陣列中所有資料**，直到陣列中的所有資料都執行完畢後，才會接著執行下方的程式碼。

#### forEach 陣列應用

以下為簡易的 forEach 陣列操作範例。

**範例一**

```js
// 數值累加
let data = [10,5,30,12];
let calcNum = 0;
data.forEach(function(item, index) {
  calcNum += item;
});
console.log(calcNum);
// 輸出結果為 57
```

如上述範例所示，宣告變數 `calcNum` 且值為 `0`，此時當 `data` 透過 `forEach` 執行第一次時，變數 `calcNum` 的值會加上陣列 `data` 中的第一個數值 `10`，接著相同的動作再進行第二次，依此類推，最終完成迴圈後的加總結果為 `57`。

接下來的範例邏輯大同小異，因此不另外做說明。

**範例二**

```js
// 計算 data 中所有偶數的加總
let data = [1,2,3,4,5,6,7,8,9,10];
let evenTotal = 0;
data.forEach(function(item, index) {
  if( item % 2 == 0 ) {
    evenTotal  += item;
  }
});
console.log(evenTotal);
// 輸出結果為 30
```

**範例三**

```js
// 加總所有學校的學生人數
let school = [
  {
    name: "學校A",
    studentNum: 35
  },{
    name: "學校B",
    studentNum: 32
  }
];
let studentTotal = 0;
school.forEach(function(item, index) {
  studentTotal += item.studentNum;
})
console.log(studentTotal);
// 輸出結果為 67
```

**範例四**

```js
// 計算及格與不及格人數
let data = [
  {
    name: 'Marry',
    sex: 'girl',
    score: 85
  },{
    name: 'Leo',
    sex: 'boy',
    score: 59
  },{
    name: 'Alvin',
    sex: 'boy',
    score: 90
  },{
    name: 'Jack',
    sex: 'boy',
    score: 48
  },{
    name: 'sophia',
    sex: 'girl',
    score: 75
  }
];
let boyPass = 0;
let girlPass = 0;
data.forEach(function(item, index) {
  if( item.sex == 'boy' && item.score >= 60 ) {
    boyPass ++;
  }else if( item.sex == 'girl' && item.score >= 60 ){
    girlPass ++;
  }
});
console.log(`男生及格人數 ${boyPass} 人，女生及格人數 ${girlPass} 人`);
// 輸出結果為 男生及格人數 1 人，女生及格人數 2 人
```

**範例五**

```JS
// 篩選出免費與投幣式的充電站
let data = [
  {
    name: 'A充電站',
    charge: '投幣式'
  }, {
    name: 'B充電站',
    charge: '投幣式'
  }, {
    name: 'C充電站',
    charge: '免費'
  }
];
let newData = { // 整合新的資料
  pay: [],
  free: []
};
data.forEach(function(item, index) {
  if( item.charge == '投幣式' ) {
    newData.pay.push(item.name);
  }else{
    newData.free.push(item.name);
  }
});
console.log(newData);
console.log(`收費充電站總共 ${newData.pay.length} 個，免費充電站總共 ${newData.free.length} 個`);
// 輸出結果 ----
// {pay: Array(2), free: Array(1)}
// free: ['C充電站']
// pay: (2) ['A充電站', 'B充電站']
// [[Prototype]]: Object
// 收費充電站總共 2 個，免費充電站總共 1 個
// ----
```

**範例六**

```html
<!-- HTML -->
<ul class="list">
  <li></li>
</ul>
```

```js
// js
let cityStatus = [
  {
    city: "高雄",
    state: "晴天"
  },{
    city: "台南",
    state: "下雨"
  },{
    city: "台北",
    state: "下雨"
  }
];
function init() { // 初始化（預設載入）
  const list = document.querySelector('.list');
  let str = '';
  cityStatus.forEach(function(item, index) {
    let content = `<li>${item.city}目前${item.state}。</li>`
   	str += content;
  });
  list.innerHTML = str;
  console.log(list.textContent); // 測試
}
init(); // 網頁載入時執行

// 輸出結果為 高雄目前晴天。台南目前下雨。台北目前下雨。
```

有時候會希望網頁載入時，某些程式碼就立即執行（如載入伺服器資料等），即初始化，此時可以參考上述範例 `init()` 的做法。

---

## AJAX

AJAX（Asynchronous JavaScript and XML）是一種非同步的 JavaScript 與 XML 技術，主要的功用是能夠讓網頁在更新內容時，不需要重新載入整個頁面，達到網址不需要變動就能夠更新局部內容效果。

### 網路請求

網路請求（HTTP Request）簡單來說，就是使用者向伺服器發出請求後，伺服器經驗證再從資料庫取得資料，並提供給使用者的過程。以瀏覽器來說，輸入網址並按下 Enter，就屬於網路請求的一種。















---
