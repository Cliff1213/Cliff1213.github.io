---
title: JavaScript 套件 - Swiper
date: 2022-06-01
tags:
 - Swiper
categories: JavaScript
---

這篇內容記錄 Swiper 套件的常見配置。

<!--more-->

---

## Swiper 套件

[Swiper](https://swiperjs.com/get-started) 是一個製作網頁輪播效果的 JavaScript 插件。

## 安裝

### CDN

透過 CDN 方式安裝 Swiper 時，需要同時引入 `swiper-bundle.min.css` 與 `swiper-bundle.min.js` 兩個檔案，引入壓縮後的版本即可。

```html
<!-- In head -->
<link rel="stylesheet" href="https://unpkg.com/swiper@8/swiper-bundle.min.css"/>
<!-- In front of body end tag -->
<script src="https://unpkg.com/swiper@8/swiper-bundle.min.js"></script>
```

### NPM

```bash
$ npm install swiper
```

---

## 基礎結構

### HTML

使用 Swiper 時，`swiper-container`、`swiper-wrapper`、`swiper-slide` 三者為必要存在的元素。

```html
<!-- Slider main container -->
<div class="swiper-container">
  <!-- Additional required wrapper -->
  <div class="swiper-wrapper">
    <!-- Slides -->
    <div class="swiper-slide">Slide 1</div>
    <div class="swiper-slide">Slide 2</div>
    <div class="swiper-slide">Slide 3</div>
    ...  <!-- 新增 swiper-slide 數量 -->
  </div>
  <!-- If we need pagination -->
  <div class="swiper-pagination"></div>

  <!-- If we need navigation buttons -->
  <div class="swiper-button-prev"></div>
  <div class="swiper-button-next"></div>

  <!-- If we need scrollbar -->
  <div class="swiper-scrollbar"></div>
</div>
```

需要進行輪播的內容，會放在 `swiper-slide` 之中，數量可依需求自行新增。

### JavaScript

Swiper 在初始化時，會帶入一個 DOM / CSS Selector，以及一個參數（字串），如下：

```js
let swiper = new Swiper( swiperContainer, parameter );
```

官方範例如下：

```js
const swiper = new Swiper(".swiper-container", {
  // Optional parameters / 加入參數與設定值
  direction: "vertical", // 輪播方向
  loop: true, // 重複顯示
  // If we need pagination / 是否顯示分頁
  pagination: {
    el: ".swiper-pagination"
  },
  // Navigation arrows / 是否加入上、下一頁方向圖示
  navigation: {
    nextEl: ".swiper-button-next",
    prevEl: ".swiper-button-prev"
  },
  // And if we need scrollbar / 是否顯示滾動軸
  scrollbar: {
    el: ".swiper-scrollbar"
  }
});
```

### 設定容器寬度

可自訂意義 Swiper 容器大小，範例如下：

```css
.swiper {
  width: 600px;
  height: 300px;
}
```

---

## 常見參數

除了先前官方範例提到的參數之外，以下列出一些在不同的版型下，可能會使用到的參數設定，更多可使用的參數可查閱[官方文件](https://swiperjs.com/swiper-api)。

**`effect`**

主要用來變更輪播時的轉場效果，預設值為 `slide`，其他可使用效果還有 `fade`、`cube`、`coverflow`、`flip`、`creative`、`cards`。

**`autoplay`**

該參數屬於一個物件，會依據給予的屬性設定來自定義自動輪播的形式，範例如下：

```js
const swiper = new Swiper(".swiper-container", {
  autoplay: {
    delay: 5000, // 自動輪播延遲時間
    disableOnInteraction: false, // 手動滑動後，停止自動撥放，預設值為 true
  }
});
```

**`slidesPerView`**

該參數可以決定輪播時，同時顯示的 `swiper-slide` 數量，範例如下：

```js
const swiper = new Swiper(".swiper-container", {
  slidesPerView: 3 // 同時顯示 3 個 swiper-slide
});
```

> 加入參數 `slidesPerView` 後，`swiper-slide` 的寬度會根據 swiper-container 來均分。

**`breakpoints`**

加入該參數後，配合參數 `slidesPerView` 可根據所設定的斷點來達成響應式的效果，範例如下：

```js
const swiper = new Swiper('.swiper', {
  // 預設的顯示數量為 1
  slidesPerView: 1,
  // 以下為斷點設定
  breakpoints: {
    // 當頁面寬度大於 768px 時，同時顯示數量為 2
    768: {
      slidesPerView: 2
    },
    // 當頁面寬度大於 992px 時，同時顯示數量為 3
    992: {
      slidesPerView: 3
    }
  }
});
```

**`spacebetween`**

該參數可以設定每個 `swiper-slide` 之間的間隙，範例如下：

```js
const swiper = new Swiper('.swiper', {
  slidesPerView: 2,
  spacebetween: 16, // 預設間隔為 20px
  breakpoints: {
    768: {
      slidesPerView: 3,
      spacebetween: 24 // 當頁面寬度大於 768px 時，間隔為 24px
    }
  }
});
```
---
