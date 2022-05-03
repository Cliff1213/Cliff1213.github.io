---
title: Bootstrap5 即時筆記（待刪除）
date: 2022-04-27
tags:
categories:
---


本篇是為了讓我練習 Bootstrap5 而即時撰寫的筆記，一段時間後會轉為草稿或刪除，標籤、文章分類部分沒有添加；內容雜亂沒有整理和反覆校正，不適閱讀請轉道。

<!--more-->

## Bootstrap 官方文件

官網中導覽列可以看到 Home / Docs / Examples / Icons / Expo / Blog 以上分類，本篇重點著重在 Docs 文件內容。

### 文件簡介

- Getting Started（快速開始）
- Customize（自定義）
- Layout（排版）
- Content（內容）
- Form（表單）
- Components（元件）
- Helpers（幫助）
- Utilities（通用類別）
- Extend（擴增）
- About（關於）

上述任意項目中，除了官方提供的範例以外，也可以透過點選頁面右邊的相關細項（On this page），直接滑動到想要尋找的內容。

### Bootstrap Icons

屬於一個獨立的套件，不需要載入 Bootstrap 即可使用。

### 引入 Bootstrap

以 CDN 方式引入為例，若是要單純使用樣式，只需要在 `<head>` 中加入以下內容：

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
```

Bootstrap 中，會有許多 `data-bs` 開頭的功能，只有上述內容是無法使用 JS 相關功能的，因此需要在 `<body>` 中加入以下內容：

```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
```

> 官方建議將 Bootstrap 的 Javascript script 標籤，放置在 body 結尾標籤之前。

### HTML 標籤中的 Class

Bootstrap 有著大量的 Class 屬性名稱，分別都代表不同的結構、樣式或狀態等，範例如下：

```html
<button type="button" class="btn btn-primary">Primary</button>
```

從上述範例中可以看到，`<button>` 套用了兩個樣式，分別為 `btn`（結構） 與 `btn-primary`（樣式）。

補充說明：

也可以使用下面的方法，直接在 html 標籤中加入 Bootstrap 的變數名稱來設定樣式。

```html
<button type="button" style="background-color: var(--bs-primary)">Primary</button>
```

## Reboot（重置）

### CSS Reset

由於 Border、Padding 會影響元素運算後的寬度，而 Bootstrap 預設就已經載入 `box-sizing: border-box;` 屬性，因此元素定義的寬度會等於實際呈現的寬度。

> Bootstrap 5 預設的 Reset 設定是使用 Normalize。

### CSS Variables

```html
<!-- HTML -->
<div class="box bg-primary"></div>

<div class="local">
  <div class="box bg-primary"></div>
</div>
```

```css
/* css */
.box {
  width: 150px;
  height: 150px;
  border: 10px solid #ddd;
  margin-bottom: 10px;
}

.bg-primary {
  background: var(--primary);
}
```

上述為範例原始碼，而 CSS 變數的定義方式分為以下兩種（兩者位置皆在上方 css 區域 .box 之前）：

**定義於全域**

```css
:root {
  --primary: #00cc99;
}
```

透過上述方法定義的變數，可以在任何區域呼叫，並且所有元素皆可以套用。

**定義於區域（進階）**

```css
.local {
  --primary: #0099cc;
}
```

定義在某區域（範圍）內的變數，只有該區域內的元素可以使用該變數，以上述原始碼為例，`--primary: #0099cc;` 只有 local 的內層元素可以進行套用。

### 單位 em / rem

以下為範例原始碼：

```html
<!-- HTML -->
<div class="em-2">
    Lorem...
    <strong class="em-2">Lorem...</strong>
</div>

<div class="rem-2">
    Lorem...
    <strong class="rem-2">Lorem...</strong>
</div>
```

```css
/* css */
.em-2 {
    font-size: 2em;
}
.rem-2 {
    font-size: 2rem;
}
```

**em**

網頁文字的預設大小為 16px，而屬性值 2em 表示兩倍的文字大小，因此 HTML 第 3 行的文字大小為 32px；而 em 的特性就是文字大小會繼承父層的文字尺寸，再重新計算，因此 HTML 第 4 行的文字大小會是 64px（32px * 2）。

**rem**

rem 文字大小的計算方式，是根據最外層（html / :root）的文字大小來決定，可以理解成網頁預設的文字大小是多少，1rem 就會是多少，因此 HTML 第 8 行的文字大小為 32px（16px * 2）。

### 系統預設字體

以下為不同作業系統上的字體設定：

```tex
作業系統 | 預設字體 | 英文字體 | 中文字體
Windows | 無 | Segoe UI | Microsoft JhengHei
Mac OS | -apple-system | San Francisco / Helvetica Neue | PingFang / Heiti TC
iOS | -apple-system | San Francisco / Helvetica Neue | PingFang / Heiti TC
Android | 無 | Roboto | Noto Sans
```

而 Bootstrap 的預設字體設定如下：

```scss
$font-family-sans-serif:
    // 使用系統預設的字體
    system-ui,
    -apple-system,
    // 指定已知的系統 UI 字體
    BlinkMacSystemFont,
    "Segoe UI",
    Roboto,
    "Helvetica Neue", Arial,
    "Noto Sans",
    "Liberation",
    // CSS 定義字體、Emoji
    sans-serif,
    "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji" !default;
```

因為 Bootstrap 只針對預設字體與英文字體做設定，因此中文字體相關設定（如：微軟正黑體）就需要手動加入。

## Typography（文字排版）

### 標題

**標題與標題文字**

雖然可以透過 `<h1>`～`<h6>` 的方式來設定文字大小，但是有時候只是想單純設定文字大小，而不是定義文字在頁面中的重要程度時，可以使用 class 名稱來取代使用標籤，以 `<h1>` 為例，`<h1>` 是表示頁面中最重要的內容，而如果只是想使用 `<h1>` 的文字大小時，設定方式如下：

```html
<!-- 錯誤方式 -->
<h1>我是一段文字</h1>

<!-- 正確方式 -->
<p class="h1">我是一段文字</p>
```

同理，雖然 `<h1>` 有預設的文字大小，但是也可以透過想同的做法來設定 `<h1>` 的文字大小：

```html
<h1 class="h2">我是一段文字</h1>
```

**小標題**

如果希望標題段落有大、小標題的區別，可使用 `<small>` 標籤來設定，以下為範例原始碼：

```html
<h3>
    我是小標題
    <small>小標題的副標題</small>
</h3>
```

> `<small>` 標籤中的文字大小為 0.875rem。

**顯示標題**

若不滿意預設的 `<h1>`～`<h6>` 文字大小，也能透過 class 的屬性 `display-1`～ `display-6` 來設定較大文字大小，以下為範例原始碼：

```html
<h1 class="display-1">我是標題</h1>
<h2 class="display-2">我是標題</h2>
<h3 class="display-3">我是標題</h3>
```

### 對齊

**文字對齊**

Bootstrap 5 的文字對齊方向邏輯如下：

- start = 靠左、從左到右
- end = 靠右、從右到左
- center = 置中

以下方原始碼為例：

```html
<div class="text-start">文字靠左</div>
<div class="text-end">文字靠右</div>
<div class="text-center">文字置中</div>
```

### 列表

**列表樣式**

可使用下列類別，變更列表樣式：

```html
<ul class="list-unstyled">
    <li>列表一</li>
    <li>列表二</li>
    <li>列表三</li>
</ul>
```

> 此類設定僅適用個別 ul。

**列表行內**

列表中的每個 `<li>` 預設的型態為區塊元素，若希望內容項目以並排的方式呈現，可以使用以下類別來設定：

```html
<ul>
    <li class="list-inline-item">列表一</li>
    <li class="list-inline-item">列表二</li>
    <li class="list-inline-item">列表三</li>
</ul>
```

> 上述 `<li>` 在並排後，彼此之間預設會有 0.5rem 的外距。

## 圖片（Images）

### 響應式

Bootstrap 5 的圖片預設並沒有響應式的功能，此時可使用 `img-fluid` 類別，讓單一圖片能夠隨著瀏覽器寬度自適應縮放大小：

```html
<img src="images/img.png" class="img-fluid" alt="...">
```

### 圖片樣式

**設定圓角**

```html
<img src="images/img.png" class="rounded" alt="...">
```

**圖片縮略圖**

在圖片標籤上使用 `img_thumbnail` 類別後，圖片就會呈現出 1px 的邊框。

```html
<img src="images/img.png" class="img_thumbnail" alt="...">
```

### 圖片置中

圖片預設的型態為 `display: inline-block;` ，因此可使用下方兩種方式，使圖片至中對齊：

**mx-auto**

```html
<img src="images/img.png" class="mx-auto" alt="...">
```

**text-center**

```html
<div class="text-center">
    <img src="images/img.png" alt="...">
</div>
```

> `text-center` 須設定在圖片的父層元素才有置中效果。

### 圖片區

如果想幫圖片加上文字描述，可以使用 `figure` 標籤，並在標籤中設定 `figure` 類別，而內層則是會分別放入 `<img>` 與 `<figcaption>` 兩個子元素，最後在後者加上 `figure-caption` 類別即可完成該圖片區，以下為範例原始碼：

```html
<figure class="figure">
    <img src="images/img.png" class="figure-img" alt="...">
    <figcaption class="figure-caption">圖片的描述</figcaption>
</figure>
```

## 表格（Form）

### 基本結構

如果需要使用 Bootstrap 5 表格，`table` 標籤內層結構必須存在 `<thead>`、`<tbody>` 兩個元素，但是無論有沒有引入 Bootstrap，在表格上會建議一律使用完整的表格架構去設計表格，範例原始碼如下：

```html
<table class="table"> <!-- 必要 -->
    <thead> <!-- 必要 -->
        <tr>
            <th scope="col">#</th>
            <th scope="col">姓名</th>
            <th scope="col">購買項目</th>
        </tr>
    </thead>
    
    <tbody> <!-- 必要 -->
        <tr>
            <th scope="row">1</th>
            <td>皮卡丘</td>
            <td>雷之石</td>
        </tr>
        
        <tr>
            <th scope="row">2</th>
            <td>小智障</td>
            <td>冠軍獎盃</td>
        </tr>
    </tbody>
    
    <tfoot> <!-- 視內容需求 -->
        <tr>
            <td>#表尾</td>
            <td>姓名表尾</td>
            <td>購買項目表尾</td>
        </tr>
    </tfoot>
</table>
```

上述範例 `<thead>` 中，`<th>` 標籤內使用的 `scope= "col"` 表示一橫列群組（第 4 行～第 6 行）；反之，`<tbody>` 中 `<th>` 標籤內所使用的 `scope= "row"` 表示一直欄群組，最後需要在最外層 `<table>` 標籤中加入類別 `table`，才能正確套用 Bootstrap 表格樣式。

### 表格色彩

**使用情境色彩**

Bootstrap 預設的情境色彩變數共有下列幾種：

```css
/* variable */
$theme-colors: (
	"primary":    $primary,
	"secondary":  $secondary,
 	"success":    $success,
  	"info":       $info,
	"warning":    $warning,
	"danger":     $danger,
	"light":      $light,
	"dark":       $dark
);
```

可依據喜好，在不同的情境下使用上述變數設定，以表格為例，如果要將整個表格的顏色變更為 `$primary`，可在 `<table>` 標籤中加入以下類別設定：

```html
<table class="table table-primary">
	...
</table>
```

**加上條文行色彩**

除了主題色，也可以在 `<table>` 標籤中加入類別 `table-striped` 來使內容區隔更加明顯：

```html
<table class="table table-striped">
	...
</table>
```

### **表格 Hover 效果**

如果要設定表格的鼠標滑入效果，只要在 `<table>` 標籤中加入類別 `table-hover` 即可，範例原始碼如下：

```html
<table class="table table-hover">
	...
</table>
```

### **啟用樣式**

在表格任意對應的標籤上加上類別 `table-active`，該欄位就會呈顯啟用狀態的效果，範例原始碼如下：

```html
<table class="table table-primary">
	<thead>
        <tr class="table-active">
            <th scope="col">#</th>
            <th scope="col">姓名</th>
            <th scope="col">購買項目</th>
        </tr>
        ...
    </thead>
</table>
```

### **表格邊框**

**加上表格邊框**

如果要加入表格邊框，可以在 `<table>` 標籤中加入類別 `table-bordered` 即可，範例原始碼如下：

```html
<table class="table table-bordered">
	...
</table>
```

**設定邊框顏色**

在以加上邊框設定的表格上，可使用 `boder-顏色變數` 的方式來設定邊框顏色，範例原始碼如下：

```html
<table class="table table-bordered border-dark">
	...
</table>
```

### 小表格

若覺得預設的表格太大，可以在 `<table>` 標籤中加入類別 `table-sm`，此時表格就會相對較小，範例原始碼如下：

```html
<table class="table table-sm">
	...
</table>
```

### 表格文字對齊

表格內容文字的對齊方式，也可以使用通用類別來設定，以最常使用的垂直置中為例，範例原始碼如下：

```html
<table class="table align-middle">
	...
</table>
```

### 表格標題

在表格中，`<thead>` 並不一定會是表格的標題，因此當表格的標題需要另外呈現時，可以在 `<table>` 的子層加入標籤 `<caption>`，範例原始碼如下：

```html
<table class="table align-middle">
    <caption>表格標題文字</caption>
    <thead>...</thead>
    ...
</table>
```

> 表格標題文字預設位置在表格下方，若想要變更位置到表格上方，可在 `<table>` 標籤中加入類別 `caption-top`。

### 響應式表格

當表格內容項目過多時，可能會超出表格寬度範圍，因此若要使表格具有響應式功能，建議在 `<table>` 標籤外新增一父層，並加上類別 `table-responsive`，範例原始碼如下：

```html
<div class="table-responsive">
    <table class="table">
		...
	</table>
</div>
```

---

## 斷點（Break Point）

表示網頁在特定的寬度下，會呈現不同的樣式，以下是 Bootstrap 5 的各個斷點：

| Breakpoint        | Class infix | Dimensions |
| ----------------- | ----------- | ---------- |
| X-Small           | *None*      | <576px     |
| Small             | sm          | ≥576px     |
| Medium            | md          | ≥768px     |
| Large             | lg          | ≥992px     |
| Extra large       | xl          | ≥1200px    |
| Extra extra large | xxl         | ≥1400px    |

### 隱藏內容

只要在任意標籤中加入類別 `d-none`，即可隱藏該標籤的所有內容，配合上述的類別名稱，可在不同的斷點設定不同的樣式，範例如下：

```html
<div class="box d-none">斷點一</div>     <!-- 頁面寬度 < 576px 時，隱藏內容 -->
<div class="box d-md-none">斷點二</div>  <!-- 頁面寬度 > 768px 時，隱藏內容 -->
<div class="box d-lg-none">斷點三</div>  <!-- 頁面寬度 > 992px 時，隱藏內容 -->
```

### 行動優先

Bootstrap 5 屬於行動優先，以類別 sm 為例，當頁面寬度 "大於或等於 576px" 時，才會套用樣式設定，因此在設計排版時需要特別注意，需要**先思考行動版，再補桌機版**，範例如下：

```html
<!-- box 內容在行動版上隱藏，桌面版顯示 -->
<div class="box d-none d-lg-block">...</div>
```

上述表示 box 預設為隱藏（行動版），但是在桌面版時，內容就會顯示。

> 小訣竅：無論如何，請先以行動版做思考。

## 容器（Container）

Bootstrap 中，Container 主要是用來定義最外層容器的寬度，在用上分別有 `container-fluid`（滿版寬度）、`container`（階段固定寬度）兩種類型。

### Container-fluid

類別名稱 `container-fluid` 的寬度會隨者視窗大小自適應伸縮，在使用上不會去設定固定寬度，在不需要嚴謹的定義最大寬度時，就可以使用。

### Container

若對於網頁內容寬度的階段美感都很要求，且需要最大寬度限制時，可使用類別 `container`。

## 欄（Column）

欄在使用上通常與 `container`、`row` 兩者脫離不了關係，範例原始碼如下：

```html
<div class="container">
    <div class="row">
        <div class="col-4">
            <div class="box">...</div>
        </div>
        <div class="col-4">
        	<div class="box">...</div>
        </div>
        <div class="col-4">
        	<div class="box">...</div>
        </div>
        
        <div class="col-4">
        	<div class="box">...</div>
        </div>
    </div>
</div>
```

上述範例中，容器 `container` 內層為 `row`，主要是讓 `row` 內層的元素能夠並排（一列）顯示，且當元素超出 `container` 寬度時，該元素會自動換行；Bootstrap 中，`container` 預設欄位數為 12 欄，因此範例中第 3 行～第 11 行的 `box` 會因為寬度加總剛好等於 12 而並排成一列，第 14 行的 `box` 則會超出寬度而自動換行。

### 等寬欄

若 `col-` 後方未加上任何數字，該 `col` 的寬度就會是剩餘空間的寬度，如果 `col` 存在數個，每個 `col` 會等寬併排成一列，因此如果確定 `col` 數量且要設計成等寬的形式時，`col-` 後方可以不需要加上數字。

### 彈性寬度

若使用類別 `col-auto` 設定寬度，該欄位就會根據該欄位的內容彈性調整寬度。

### 朝狀

欄位中也可以再加入內層的欄位，形成朝狀結構，範例如下：

```html
<div class="container"> <!-- 必要加入 -->
	<div class="row">
        <div class="col-6">
            <div class="row"> <!-- 必要加入 -->
                <div class="col-4">...</div>
                <div class="col-4">...</div>
                <div class="col-4">...</div>
            </div>
        </div>
        <div class="col-6">
            ...
        </div>
    </div>
</div>
```

> `row` 內層必須是 `col`，且只能存在 `col`，而 `col` 內層可以再使用 `row` 來新增 `col` 排版，但最外層必須是容器 `container` 或是 `container-fluid`。

### 響應式欄

欄位相關類別也可以搭配斷點進行響應式設計，範例如下：

```html
<div class="container">
    <div class="row">
        <div class="col-6 col-md-4 col-xl-3">...</div>
        <div class="col-6 col-md-4 col-xl-3">...</div>
        <div class="col-6 col-md-4 col-xl-3">...</div>
        <div class="col-6 col-md-4 col-xl-3">...</div>
    </div>
</div>
```

上述範例表示手機版欄位會呈現三個並排，平板時會兩個並排，桌面板則會四個並排。

> 當有存在其他中斷點時，可以不用加上類別 `col-12`。

## 欄間距（Gutter）

### Gutter 運作概念

Bootstrap 中，每個 `col-*` 都有預設的左右 padding，而 gutter 就是由兩個 `col-*` 之間的 padding 相加所產生的，因此一個 `col-*` 實際的寬度會包含兩側 padding，但內容會因為兩側的 padding 而貼齊 gutter 邊線。

延續上述內容，因為每個 `col-*` 兩側都有獨立的 padding，因此容器 `container` 內部的左右兩側會多出一個 `col-*` 的 padding，即 gutter on outside，而 container 本身就有自己的 padding，所以這會讓內容顯得往內縮，因此 `row` 就會加上 `margin-left`、`margin-right` 的負數值，來消除 gutter on outside。

Gutter 是透過變數產生的，因此可以修改，如果要修改 gutter 數值，可以透過 `gx-*` 來調整，可使用數值為 0～5，範例如下：

```html
<div class="container">
    <div class="row gx-5"> <!-- 預設為 1.5rem -->
        <div class="col-4">...</div>
        <div class="col-4">...</div>
        <div class="col-4">...</div>
    </div>
</div>
```

實際較常會運用在相近內容的群組上，範例如下：

```html
<div class="container">
    <div class="row">
        <div class="col-6">...</div>
        
        <div class="col-6">
            <div class="row gx-2">
                <div class="col-4">...</div>
                <div class="col-4">...</div>
                <div class="col-4">...</div>
            </div>
        </div>
    </div>
</div>
```

上述範例第 6 行因為加上 `gx-2`，因此視覺上比較有一個群處的感覺。

> Gutter 可使用的設定分別有 g-（水平與垂直）、gx-（水平）、gy-（垂直），也可以搭配中斷點設定；另外如果需要移除 gutter 時，可以將數值設定為 0。

### 行列式

除了可以在元素上加上 `col-*` 來設定欄數，也可以使用 `row-cols-{ 欄數 }` 來設定，範例如下：

```html
<!-- 使用 row-cols-* -->
<div class="container">
    <div class="row row-cols-3">
        <div class="col">...</div>
        <div class="col">...</div>
        <div class="col">...</div>
        <div class="col">...</div>
    </div>
</div>

<!-- 不使用使用 -->
<div class="container">
    <div class="row">
        <div class="col-4">...</div>
        <div class="col-4">...</div>
        <div class="col-4">...</div>
        <div class="col-4">...</div>
    </div>
</div>
```

上述範例中，兩者的結果是相同的，而行列式也可搭配中斷點設定，如 `row-cols-md-3`。

## Flex 

可使用通用類別 `d-flex` 將容器加上 flex 屬性，必須加上該設定才能使用 flex 相關延伸設定，而 flex 有兩種形式呈現，範例如下：

```css
d-flex > 區塊會以滿版呈現
d-inline-flex > 區塊寬度會根據內容決定（較少使用）
```

### 主軸

預設的軸線方向設定為 `flex-row`（由左至右），可使用 `flex-row-reverse` 來反轉起始方向，內容可使用 `justify-content` 來設定對齊位置，起點表示 `start`、終點表示 `end`。

### 交錯軸

預設方向為由上而下，與主軸垂直交錯，內容可使用 `align-items` 來設定內容對齊位置。

### 設定軸線方向

使用 `flex-column` 可將軸線方向變更為由上往下。

### 內容水平對齊

水平對齊的方式，分為下列幾種：

- justify-content-start（靠左對齊）
- justify-content-end（靠右對齊）
- justify-content-center（水平置中對齊）
- justify-content-between（左右對齊）
- justify-content-around（兩物件水平間距相等）
- justify-content-evenly（水平間距皆相等）

### 內容垂直對齊

垂直對齊的方式，分為下列幾種：

- align-items-start（靠上對齊）
- align-items-end（靠下對齊）
- align-items-center（垂直置中對齊）
- align-items-between（上下對齊）
- align-items-around（兩物件垂直間距相等）
- align-items-evenly（垂直間距皆相等）

### 屬性

```css
/* 外層屬性 */
display /* 必備屬性 */
/* flex-flow */
　flex-direction /* 決定 flex 軸線方向 */
　flex-wrap /* 決定換行屬性 */
justify-content /* 主軸的對齊 */
align-items /* 交錯軸的對齊 */

/* 內層屬性 */
/* flex */
　flex-grow /* 伸展比 */
　flex-shrink /* 收縮比 */
　flex-basis /* 絕對值 */
order /* 排序 */
align-self /* 單一物件的交錯軸對齊 */
```

## 間距（Spacing）

間距的使用格式為 `{ property }{ sides }-{ size }`，範例如下：

```html
<h1 class="p-5">我是標題</h1> <!-- 上下左右皆增加 5 個單位的內距 -->
<h1 class="px-5">我是標題</h1> <!-- 水平方向（兩側）增加 5 個單位的內距 -->
<h1 class="ps-5">我是標題</h1> <!-- 單一方向（左側）增加 5 個單位的內距 -->
```

### 內、外距

間距的 `property` 分別可使用以下兩種設定：

- p = padding
- m = margin 

### 方向

間距的 `sides` 可使用下列幾種設定：

- s = left
- e = right
- t = top
- b = bottom
- x = left & right
- y = top & bottom

### 單位數值

間距的 `size` 表示距離的單位，1 個單位預設的距離 `$spacer` 為 1rem，即 16px，可使用的單位數值如下：

```scss
1 > （預設）設定 margin 或 padding 為 $spacer * .25
2 > （預設）設定 margin 或 padding 為 $spacer * .5
3 > （預設）設定 margin 或 padding 為 $spacer
4 > （預設）設定 margin 或 padding 為 $spacer * 1.5
5 > （預設）設定 margin 或 padding 為 $spacer * 3
auto > 設定 margin 為 auto
```

### 清除元素預設距離

Bootstrap 中部分元素預設會有自己的 padding、margin，因此若要清除該元素預設的距離，可將單位數值設定為 0 即可，以 `<p>` 標籤為例，範例如下：

```html
<p class="mb-0">我是文字內容</p> <!-- p標籤預設存在 margin-bottom: 1rem -->
```

### 負值運用

如果要將元素預設的 padding 清除，可在單位數值前面加入 `n`（負值的 margin），範例如下：

```scss
<div class="box mx-n2"></div>
```

> 該功能較少使用，因此預設是關閉的，需要透過手動調整 Bootstrap Sass 才可開啟功能。

### Auto margins

如果要將一個區塊推移至任意一側，可使用 `{ property }{ sides }-auto` 方式來設定，以下為範例：

```html
<div class="box mb-auto"></div>
```

以上範例的結果會使 box 推到最上方的位置。

### Wrap

使區塊內的元素在超出寬度時自動換行，僅可用於 flex 容器中。

### Order

如果要變更元素的排列順序，可在該元素標籤內加入 `order-順序`，僅可用於 flex 容器中。

### Align-content

如果要調整 flex 容器中所有內容的對齊方式，可使用 `align-content-` 來設定，但對單行內容無效果。

---

## 顏色（Colors）

### 連結內容顏色

`text-顏色變數名稱` 並不適合用於連結，因為連結預設的 hover 效果在內容設定 `text-顏色變數名稱` 後，會無法顯示效果，如果要設定連結文字的顏色，會使用工具的 `link-顏色變數名稱` 來設定。

## 浮動（Float）

該功能目前較少使用。

### 繞圖排文

範例原始碼如下：

```html
<p>
	<img src="images/img.png" class="float-end ms-3" alt="img">
    Lorem...
</p>
```

上述設定會使圖片靠右對齊，文字內容會繞著圖片進行排列。

### 清除浮動

元素在設定浮動後，可能會造成父元素崩塌（抓不到內容高度），因此需要在父元素加上 `clearflx` 來清除浮動，範例如下：

```html
<div class="clearfix">
    <div class="box float-start"></div>
    <div class="box float-end"></div>
</div>
```

## 互動（Interaction）

### Text-selection

相關設定如下：

- user-select-all > 文字內容被點選時，段落將會被全選。
- user-select-auto > 該文字段落具有預設的選取行為。
- user-select-none > 文字內容被點選時，該段落無法被選取。

### Pointer events

相關設定如下：

- pe-none > 該段落的所有連結無法被點選。
- pe-auto > 該段落的所有連結無法被點選，但可在部分片段加上此類別設定，使片段內的該連結可被點選。

## 位置（Position）

Bootstrap 的基礎位置設定如下所示：

- position-static
- position-relative
- position-absolute
- position-fixed
- position-sticky

以下為範例原始碼：

```html
<!-- 範例一 -->
<div class="position-relative" style="width: 200px; height: 200px;">
    <div class="box position-absolute bottom-0 end-0" style="width: 50px; height: 50px;"></div>
</div>
<!-- 範例二 -->
<div class="position-relative" style="width: 200px; height: 200px;">
    <div class="box2 position-absolute bottom-0 start-100" style="width: 50px; height: 50px;"></div>
</div>
```

上述設定中，因為設定了 `bottom-0`、`end-0`，因此 box 會移動到又下角並貼齊邊緣；而 box2 所設定的 `start-100` 會因為數值所推的距離是根據外層寬度決定（外容器寬度的100%），因此位置會被推到外層容器之外並貼齊邊緣，同理，如果將 box2 的數值修改為 `start-50`，box2 的位置會是外層容器寬度的 50%，因此並不會水平置中。

### 元素置中

如果要讓範例中的 box2 達到水平置中的效果，可以在 box2 加上 `translate-middle-x`。

> 垂直置中的設定為 translate-middle-y；水平垂直皆置中則直接設定 translate-middle。

## 尺寸（Sizing）

使用 `w-數值` 時，元素會依據外層容器來設定寬與高，以寬度設定為例，可設定數值如下：

```html
<div style="width: 100px; height: 100px">
    <div class="p- w-25"></div>
    <div class="p-5 w-50"></div>
    <div class="p-5 w-75"></div>
    <div class="p-5 w-100"></div>
    <div class="p-5 w-auto"></div> <!-- 將元素寬度變回預設狀態 -->
</div>
```

> 高度設定同理寬度設定。

### 最大寬度、高度

在元素上加入 `mw-100`，可使元素內容的寬度不會超過外層容器寬度，同理 `mh-100`。

### 相對於視窗

若希望容器的寬高和瀏覽器畫面相同時，可使用以下設定：

- wh-100 > 元素寬度等於裝置寬度
- vh-100 > 元素高度等於裝置高度

## 可視性（Visibility）

設定元素的可視性，設定方式分別有下列兩種：

- visible > 看的到元素（預設）。
- invisible > 讓元素呈現看不見的狀態，旦區塊佔用空間依然存在。

> invisible 與 d-none 兩者特性不同。

## 比例（Ratio）

若要設定元素的比例，可使用 `ratio` 相關設定，可使用的比例數值分別有以下幾種：

- 16x9
- 4x3
- 1x1
- 21x9

該設定適合用於內嵌影片，範例如下：

```html
<div class="ratio ratio-16x9"> <!-- 加上 ratio 後，才可使用 ratio-* 相關設定 -->
    <iframe src="https://..." title="Youtube video" allowfullscreen></iframe>
</div>
```

## 連結延伸（Stretched link）

若一個元素內存在一個連結，且希望鼠標點擊該元素上任意位置上，都可以觸發連結時，可以在該連結上使用工具類別 `stretched-link`，範例如下：

```html
<!-- 以下內容取自 Bootstrap 官方文件 -->
<div class="demo">
    <div class="card" style="width: 18rem;">
        <img src="images/img.png" class="card-img-top img-fluid" alt="..." >
        <div class="card-body">
          <h5 class="card-title">帶有延伸連結的卡片</h5>
          <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
          <a href="#" class="btn btn-primary stretched-link">Go somewhere</a>
        </div>
    </div>
</div>
```

在使用 `stretched-link` 時，需要留意外層是否有設定 `position-relative`，因為該設定視透過絕對定位來延伸連結範圍至外層容器，因此無法單獨使用在外層沒有相對定位的群組中，而 Bootstrap 的 Card（卡片）預設已加上相對定位的設定，因此上述範例中不額外做添加。

> Bootstrap 大多數自定義的原件並不預設帶有 `position: relative`，因此必須添加 `potition: relative`，避免連結延伸到父容器以外的區域。

## 文字截斷（Text truncation）

如果內容文字較長，可以使用工具類別 `text-truncate` 來將超出寬度的內容，透過刪節號截斷文字，並以單行文字內容呈現，但是外層容器需要限制寬度（容器型態必需是 `display: inline-block` 或 `display: block`）才可正常運作，範例如下：

```html
<!-- block -->
<div class="row">
    <div class="col-2 text-truncate">
    	Lorem100
    </div>
</div>

<!-- inline -->
<span class="d-inline-block text-truncate" style="max-width: 150px;">
	Lorem100
</span>
```

