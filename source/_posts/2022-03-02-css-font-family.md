---
title: 關於 CSS Font-Family
date: 2022-03-02
tags:
 - font-family
categories: CSS
index_img: img/banner/banner_font.jpg
---

字體設定規則有點複雜，趁現在還記得順便紀錄一下。

<!--more-->

---

## Font-Family 設定規則

基本上只要是網頁設計，都會使用到的屬性，可以自訂頁面上的文字字體，但是初學者在使用這個屬性之前，為了避免結果不如預期，建議需要先理解一些設定上的規則。

### 字體套用邏輯

Font-Family 雖然可以設定多個屬性值，但是並不會將所有寫入的字體都一起套用，而是會根據瀏覽器對該字體的支援與否，由左至右依序判斷，以下方原始碼為例：

```css
font-family: 字體A, 字體B, 字體C;
```

假設使用者的電腦系統中僅存在 "字體B"，此時瀏覽器載入網頁時，會根據上述設定先判斷系統內是否有對應的字體A，但因為系統中找不到這個字體，因此會跳過該字體並判斷下一個字體B，此時找到字體B之後，就會套用該字體，並且不再往下進行判斷。

### 語法注意事項

在部分字體的選用上，同一個字體可以選擇使用中文名稱或英文名稱的屬性值設定，其結果會相同，以下方原始碼為例：

```css
font-family: "Microsoft JhengHei";
font-family: 微軟正黑體;
```

從上述語法中可以發現，第一行的字體加上了雙引號，這是因為該字體的名稱之間如果存在空白，在不加上雙引號的情形下，瀏覽器會無法辨識而導致讀取失敗。

另一個值得注意的是，雖然使用中文名稱來設定字體結果與英文名稱相同，但是只單獨設定中文名稱的話，可能會遇到編碼的問題而導致結果不如預期，因此會建議使用英文名稱的屬性值，或是中、英文一起使用，以下方原始碼為例：

```css
font-family: "Microsoft JhengHei", 微軟正黑體;
```

---

## Font-Family 字體類型

在當今這麼大量的字體中，主要可以分為 "指定字體" 與 "通用字體" 兩種類型。

### 指定字體（family-name）

不同的字體也會影響網頁的視覺感受，因此在選用字體時，通常會優先使用指定的字體。

### 通用字體（generic-family）

通用字體可以理解成多數的電腦系統中，內建就已存在的字體，常見的通用字體有以下五種，其中無襯線體、襯線體使用最為廣泛：

- sans-serif（無襯線體 / 黑體）
- serif（襯線體 / 明體）
- cursive（手寫體）
- monospace（等寬體）
- fantasy（幻想體）

### 設定方式

雖然可以透過指定字體名稱的方式，來達到自訂網頁上想要呈現的字體，但是並非所有電腦系統都有對應的指定字體，因此在使用指定字體時，通常都會在 font-family 屬性值的**最後方位置**設定預設的通用字體；而前面有提到 font-family 是由左至右判斷屬性值，所以當瀏覽器不支援或無法從系統中找到前方所對應的指定字體時，就會套用最後設定的通用字體，設定格式如下：

```css
font-family: 指定字體, "指定 字體", 通用字體;
```



---

## 字體順序觀念

本篇一開始在設定規則的地方有簡單提到字體是由左邊開始判斷，但是在順序方面還有一些重要觀念需要熟記，這邊先記住以下設定順序，後續會說明原因：

設定順序：**英文字體 > Linux > Mac > Windows > 通用字體**
<!-- 這部分一直沒有比較好的解釋 -->



### 中、英文順序

一個網頁通常會存在多個語言（以中、英文為例），但是並非所有的字體都同時支援中、英文，因此許多網頁會利用這個由左至右判斷的技巧，分別針對中文與英文各別設定不同的字體，以下方原始碼為例：

```css
font-family: Arial, 微軟正黑體;
```

上述設定中，Arial 只有英文字體，而微軟正黑體則是中、英文字體都有，因此瀏覽器在判斷 Arial 之後，就會因為符合條件而套用英文字體，而中文會因為找不到對應字體則往下判斷微軟正黑體並套用，此時網頁上的中文與英文就會呈現不同字體的效果。

但是如果順序換成微軟正黑體在前面，瀏覽器就會先判斷微軟正黑體，因為該字體中、英文都可以使用，所以網頁上的兩種語言都會套用成微軟正黑體，而後面的 Arial 會因為英文字體已套用成微軟正黑體而不會被讀取。

因此，正確的屬性值順序為**英文字體在前面，中文字體在後面。**

### 字體使用率與順序

每個作業系統都有自己的系統字體，例如 -apple-system 與 BlinkMacSystemFont 分別是 iOS 以及 macOS 的系統字體，並且前者只能使用在 Safari 瀏覽器上，後者則是用於 Chrome，像這種只能使用在特定系統或瀏覽器的字體，順序就需要放在前面，以下方原始碼為例：

```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Microsoft JhengHei", Roboto, "Helvetica Neue", Arial, sans-serif;
	     /* mac系統字體, iOS系統字體,	windows英文系統字體, 微軟正黑體, Android系統字體, iOS系統字體, 通用字體, 通用字體 */
```

假設使用者電腦系統為 windows，瀏覽器因為在系統中找不到 -apple-system 與 BlinkMacSystemFont 而直接跳過這兩個字體，而下一個 Segoe UI 屬於 windows 的英文系統字體，因此英文字體就會套用 Segoe UI，中文字體則是因為 Segoe UI 並沒有對應的中文而判斷下一個值，最終套用 Microsoft JhengHei。

如果電腦系統是 macOS 並且使用 Chrome 瀏覽網頁，當瀏覽器在讀取到 -apple-system 時，就會套用對應的中文及英文字體，並且不再往下判斷。

因此，為了適應各種不同的瀏覽器與作業系統，在設定字體時需要把**較少使用到的字體放前面，較常使用到的字體放後面。**

---

## 實務範例

附上幾個知名企業的字體設定當作參考，有興趣的也可以自行研究順序邏輯。

Apple（台灣）：

```css
font-family: "SF Pro TC", "SF Pro Text", "SF Pro Icons", "PingFang TC", "Helvetica Neue", "Helvetica", "Arial", sans-serif;
```

微軟台灣：

```css
font-family: 'Segoe UI', SegoeUI, 'Microsoft JhengHei', 微軟正黑體, "Helvetica Neue", Helvetica, Arial, sans-serif;
```

Google：

```css
font-family: arial, sans-serif;  /* 這絕對不是懶(? */
```

---

**參考資料：**

[CSS font-family 詳細介紹](https://www.oxxostudio.tw/articles/201811/css-font-family.html)
[最標準的系統字型規範 font-family](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/180456/#outline__1_2_1)
[font-family要怎麼玩](https://www.casper.tw/css/2014/01/01/font-family/)
