---
title: Flex 水平置中，子元素溢出不被裁切
date: 2022-09-17
tags:
 - flex
 - overflow
categories: CSS
index_img: img/banner/banner_20220910-2.jpg
---

記錄一下遇到的切版問題的解法。

<!--more-->

------

前一段時間在切版時，遇到一個有趣的問題，範例如下：

![](https://i.imgur.com/TK9ZZcF.png)

```html
<!-- HTML -->
<section class="section mt-3">  
  <ul class="list">
    <li class="list-item">item</li>
    <li class="list-item">item</li>
    <li class="list-item">item</li>
    <li class="list-item">item</li>
    <li class="list-item">item</li>
    <li class="list-item">item</li>
  </ul>
  <div class="container">
    <h2 class="section-title">Section Title</h2>
    ...
  </div>
</section>
```

```css
/* CSS（上方列表區塊） */
.list {
  display: flex;
  justify-content: center;
  overflow: auto;
}
```

上述範例中，上方 `.list` 區塊使用 `display: flex` 加上 `justify-content: center` 讓子元素置中，同時希望瀏覽器寬度縮小至子元素溢出時，溢出的部分以水平軸方式呈現，因此加入了 `overflow: auto` 設定，呈現畫面如下：

![](https://i.imgur.com/Sn1BMH4.png)

然而將水平軸向左滾動至最底時，溢出的部分卻被裁切了，而問題就在 `justify-content: center` 這個屬性上，移除該屬性之後，子元素溢出的部分就會正常呈現，如下：

![](https://i.imgur.com/6GYarUd.png)

此時問題來了，在不使用 `justify-content: center` 的情況下，要如何同時滿足子元素保持置中，但是溢出情況下不會被裁切呢？在思索一番後想到另一個能使區塊水平置中的語法，就是 `margin: 0 auto`，不過這個語法只適用具有實際寬度但非滿版的區塊，而範例中的 `.list` 區塊因為 `display: flex` 的關係佔了滿版的寬，因此並沒有如預期置中。下圖是區塊實際佔用的空間：

![](https://i.imgur.com/WnJz6ud.png)

**解決方式：**

```css
/* CSS（上方列表區塊） */
.list {
  display: flex;
  max-width: max-content;
  margin: 0 auto;
  overflow: auto;
}
```

僅需在 `.list` 區塊加上 `max-width: max-content` 語法即可，而 `max-content` 最基本的作用是讓區塊寬度或高度自適應子元素，並且當子元素溢出時，內容不會受到擠壓而換行；此處正好利用了前者特性，使 `.list` 最大寬度等於所有子元素（`.list-item`）寬度的加總，此時再配合 `margin: 0 auto` 之後區塊得以置中，並且當子元素溢出時不會被裁切。結果如下圖所示：

![](https://i.imgur.com/MLip3hT.png)

**實作範例：**

{% iframe https://codepen.io/Cliff_hex/embed/bGMqLOq?default-tab=css%2Cresult %}

> max-content 詳細說明請參考此[文章](https://developer.mozilla.org/en-US/docs/Web/CSS/max-content)