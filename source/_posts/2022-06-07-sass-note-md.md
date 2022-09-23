---
title: Sass 實用語法
date: 2022-06-07
# tags:
categories: SCSS
---

簡單整理一下自己常用且覺得方便的一些 Sass 相關語法。


<!--more-->

---

## 入門技巧

### 使用 & 表示父選擇器

以下是一個簡單的 Scss 格式設定：

```scss
.header {
  text-align: center;
  .header-title {
    font-size: 2rem;
  }
  a {
    padding: 8 16px;
    background: #eee;
  }
  a:hover {
    background: #ddd;
  }
}
```

上述例子可以改用 & 的方式來撰寫，以 & 符號來表示父層，其運行結果想同，如下所示：

```scss
.header {
  text-align: center;
  &-title {
    font-size: 2rem;
  }
  a {
    padding: 8 16px;
    background: #eee;
    &:hover {
      background: #ddd;
    }
  }
}
```

### Variable 變數

一個設定可能會在許多不同的區塊重複被使用，此時就能夠透過變數來統一管理，好處是後續若需要修改值，只需要修改變數值即可，而變數格式為 `$name: value`，以顏色為範例，如下所示：

```scss
$color-primary: #00cc99;
.header {
  &-title {
    color: $color-primary;
  }
}
.footer {
  &-title {
    color: $color-primary;
  }
}
```

變數值可使用的格式分別有以下幾種：

- 數字：2、16px、2rem
- 字串："文字"、"./img/logo.png"
- 顏色：blue、#00CC99、rgba(0,0,0,0.5)
- 布林：true、false
- 空值：null
- 值列：16px 8px、"arial, sans-serif"
- 運算：1rem * 1.25

### 調整色彩明度

除了直接輸入顏色或色碼，也能夠使用 `darken`、`lighten` 來調整所設定顏色的明暗程度，範例如下：

```scss
$color-primary: #00cc99;
.title {
  color: darken($color-primary, 25%); // 亮度減少 25%
}
a {
  color: lighten(#ddd, 15%); // 亮度增加 15%
}
```

### @import 匯入檔案

較大的專案為了方便管理程式碼，通常會將 scss 檔案進行較細的拆分，而這些拆分出來的檔案可透過 `@import` 來統一匯入（合併）一個 scss 檔案中，範例如下：

```scss
// 目前檔案為 all.scss

@import 'variable';

@import 'reset';
@import 'base';

@import 'mixin';

@import 'main';
```

> 檔案的分類可參考 [Sass 7-1 Pattern](https://gist.github.com/rveitch/84cea9650092119527bc)，或是此篇[文章](https://medium.com/ivycodefive/4-%E4%BB%80%E9%BA%BC%E6%98%AFsass-7-1%E6%9E%B6%E6%A7%8B-8687e9a10a64)。

補充說明：被合併的 scss 檔案，檔案名稱會使用 _ 做開頭，除了用來表示合併用，也不會被編譯成 css 檔案，但是在匯入時不需要加上下底線，如下所示：

```scss
// 正確匯入方式
@import 'variable';
// 錯誤匯入方式
@import '_variable';
```

### @mixin 混合多個設定

Mixin 的原理與變數相同，主要是將一些較常使用的語法組合封裝並且定義成一個名稱，等需要使用時，再透過 `@include` 來導入內容，範例如下：

```scss
// _mixin.scss
@mixin textHide { // 圖片取代文字
  display: inline-block;
  text-indent: 101%;
  white-space: nowrap;
  overflow: hidden;
}

@mixin desktop { // 響應式斷點
  @media (min-width: 768px) {
    @content; // 表示導入後所撰寫的內容位置
  }
}
```

```scss
// _main.scss
h1 .logo {
  background-image: url("images/logo.svg");
  width: 150px;
  height: 80px;
  background-size: contain;
  @include textHide;
  @include desktop {
    // 此區塊的內容會對應 @content 位置
    width: 200px;
    height: 100px;
  }
}
```

---

## 進階技巧

### @for 運作原理

Sass 的 `@for` 運作方式類似 JavaScript 的 for 迴圈，能夠依序並重複套用相同的設定，範例如下：

```html
<!-- html -->
<div class="box box-1"></div>
<div class="box box-2"></div>
<div class="box box-3"></div>
<div class="box box-4"></div>
<div class="box box-5"></div>
```

```scss
// scss
.box {
  width: 100px;
  height: 100px;
}
@for $i from 1 through 5 { // through 若改使用 to，則不包含最終值
    .box-#{ $i }{ // 變數需要加上 # 並使用 {} 包覆，否則無法正常讀取
    background: darken( #eee, $i * 5% );
  }
}
```

上述範例 scss 設定中，第 6 行使用了 `@for` 方法來設定不同的背景顏色，`$i` 表示帶入數字的變數，`from` 會接一個起始值，`through` 則是會接一個最終值。

而第 7 行的 `$i` 會先帶入初始值 1，此時就會針對 `.box-1` 設定下方的樣式 `background: darken( #eee, 1 * 5% )`，接著 `$i` 會再帶入 2，並進行與前一次相同的設定，直到最終值進行結束為止。

### Sass Map 是甚麼?

Sass Map 是一種集合變數，概念有點類似 JSON，但是不同的地方在於 Sass Map 會將定義的變數透過**小括弧** `()` 包裝成一個群組，格式為 `$變數名稱: (key: value);`，範例如下：

```scss
$colors: (
  // key: value
  'primary': #007bff, // 每個設定之間需要使用半形逗號隔開
  'secondary': #6c757d,
  'success': #28a745
); // 必須加上 ; 做結尾以免影響後方程式碼運作
```

設定 Sass Map 之後，可以透過 `map-get` 來取用變數的內容，以上述例子來說，若想取用 `$primary` 來當作文字的顏色，做法如下：

```scss
h1 {
  background: map-get($colors, 'primary');
};
h2 {
  background: map-get($colors, 'secondary');
}
```

以上述範例來說，雖然可以透過 `map-get` 來取得對應的變數內容，但是若針對個別項目逐一取用設定，效率會相對較低，因此通常會搭配 `@each` 來做使用。

### @each 運作原理

Sass 的 `@each` 類似 JavaScript 的 each，搭配先前提到的 Sass Map 就可以產出大量的樣式設定，而 `@each` 的格式為 `@each $key, $value in 集合變數{...}` ，範例如下：

```html
<!-- HTML -->
<a href="#" class="btn btn-primary">Button1</a>
<a href="#" class="btn btn-secondary">Button2</a>
<a href="#" class="btn btn-success">Button3</a>
```

```scss
// scss
.btn {
  padding: 8px 16px;
}
$colors: (
  'primary': #007bff,
  'secondary': #6c757d,
  'success': #28a745
);
@each $key, $value in $colors {
  .btn-#{$key} {
    background: $value;
  }
};
```

上述範例 scss 設定中，第 10 行使用了 `@each` 並指定了集合變數 `$colors`，此時變數 `$colors` 中的每個 key 與 value 就會帶入 `@each` 大括弧裡的對應位置，最後再套用設定到對應的類別之中。

### @extend 合併相同樣式

在進入 `@extend` 之前，先簡單說明**佔位符選擇器**（Placeholder Selectors）是什麼，它與 class、id 兩種選擇器類似，差別在於佔位符選擇器在 Sass 中會使用 `%` 來定義，且必須透過 `@extend` 來調用，在被 `@extend` 調用之前，本身並不會被編譯到 .css 檔案中。

進入正題，如果相同的樣式設定被重複使用在多個類別中，可以透過 `%` 搭配 `@extend` 的做法，使 Sass 檔案在編譯後，有相同設定的類別就會被合併在一起，以減少多餘的程式碼產生，簡單概念如下：

```scss
// .scss 未使用 @extend
.header-btn {
  display: inline-block;
  padding: 8px 16px;
  background: #007bff;
}
.section-btn {
  display: inline-block;
  padding: 8px 16px;
  background: #6c757d;
}
.footer-btn {
  display: inline-block;
  padding: 8px 16px;
  background: #28a745;
}
```

```scss
// .scss 使用 @extend
%btn-base {
  display: inline-block;
  padding: 8px 16px;
}
.header-btn {
  @extend %btn-base;
  background: #007bff;
}
.section-btn {
  @extend %btn-base;
  background: #6c757d;
}
.footer-btn {
  @extend %btn-base;
  background: #28a745;
}
```

```css
/*  編譯後的 .css  */
.footer-btn, .section-btn, .header-btn { /* 相同設定合併在一起 */
  display: inline-block;
  padding: 8px 16px;
}
.header-btn {
  background: #007bff;
}
.section-btn {
  background: #6c757d;
}
.footer-btn {
  background: #28a745;
}
```

可能會有人說，直接將共通的設定寫在一個 `.btn` 裡面不是比較方便且簡潔嗎？確實是這樣，而上述使用 `%btn-base` 來當作案例，主要是為了方便理解 `@extend` 的運作原理，實際上較不會透過以上做法來設定按鈕的共通樣式。

---

**參考資料：**

[什麼是Sass 7-1架構?](https://medium.com/ivycodefive/4-%E4%BB%80%E9%BA%BC%E6%98%AFsass-7-1%E6%9E%B6%E6%A7%8B-8687e9a10a64)
[鐵人賽 25 - 實戰心法 - Sass Map 快出產出大量樣式](https://www.casper.tw/css/2016/12/25/sass-map/)