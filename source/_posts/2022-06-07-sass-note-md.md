---
title: Sass 之你必須知道的實用技巧
date: 2022-06-07
tags:
 - Sass
 - css
categories: CSS
---

整理一下自己常用且覺得方便的一些 Sass 相關語法，不會討論編譯工具或其他非相關內容。

> 本篇會以 Scss 格式來做說明。

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

上述例子可以改用 & 的方式來撰寫，以 & 符號來表示父曾，其運行結果想同，如下所示：

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
- 字串："文字內容"、"./images/logo.png"
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

### @extend 繼承



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
  .box-#{ $i }{
    background: darken( #eee, $i * 5% );
  }
}
```

上述範例 scss 設定中，第 6 行使用了 `@for` 方法來設定不同的背景顏色，`$i` 表示帶入數字的變數，`from` 會接一個起始值，`through` 則是會接一個最終值。

而第 7 行的 `$i` 會先帶入初始值 1，此時就會針對 `.box-1` 設定下方的樣式 `background: darken( #eee, 1 * 5% )`，接著 `$i` 會再帶入 2，並進行與前一次相同的設定，直到最終值進行結束為止。

### @each 運作原理

Sass 的 `@each` 類似 JavaScript 的 each，搭配 Sass Map 可以產出大量的樣式設定，範例如下：

```html

```

```scss

```



---

## 參考資料

- [什麼是Sass 7-1架構?](https://medium.com/ivycodefive/4-%E4%BB%80%E9%BA%BC%E6%98%AFsass-7-1%E6%9E%B6%E6%A7%8B-8687e9a10a64)