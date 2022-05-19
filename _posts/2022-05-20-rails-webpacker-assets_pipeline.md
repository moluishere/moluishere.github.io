---
title: "【Ruby】每天一點 Rails：使用JS套件的引入方法 - Webpacker & Assets Pipeline "
date: 2022-05-20
categories:
  - blog
tags:
  - Rails
  - Assets Pipline
  - Webpacker
---

 關於 rails 中使用JS第三方套件，Webpacker 與 Assets Pipeline 的引入方法

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/1400/0*w2HdkZxq7ABkKPrO.jpeg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">photo by drrpal</div>
</center>

> 前提：初學者的學習筆記，僅供參考，敬請指教～

本文使用 Rails 6，搭配 Stimulus JS，並以安裝 `Huebee` 為範例。

## 靜態檔案
Rails 專案的開發中，會使用到許多的靜態的檔案。
所謂的靜態檔案是指瀏覽器呈現畫面所使用到的資源檔案，例如 JavaScript、CSS、圖檔、影像檔等等。

Rails 目前提供了兩種方式進行靜態檔案的管理：

1. Webpacker
2. Assets Pipeline

Rails 6 預設使用 Webpacker， 並且可以和 Assets Pipeline 一起使用。 Rails 7 後則回歸到 Assets Pipeline 處理。

我們在使用JS的套件時，除了JS檔本身，應該也會有CSS檔，這時候該使用什麼方式引入呢？

--

在進行專案開發時，有一個功能為是讓使用者自己選擇主題顏色，筆者找到一個 JS 套件可以實現這個選色功能。一開始對於引入的方法不熟悉，直接在最上層的 layout 使用 CDN 引入。對於預設 Webpacker 而有著肥大 `node_modules` 的 Rails 6 來說， 並不是一個（待補）

1. JS 掛載
2. 看官方說明 -> 在 JS 中引入（import ）JS檔案
3. 在 JS 中引入 CSS 檔 OR 在 assets 中的 stylesheets/application.css 引入





參考文章：
+ [如何在 Rails 使用 Webpacker（上）](https://kaochenlong.com/2019/11/21/webpacker-with-rails-part-1/)
+ [如何在 Rails 使用 Webpacker（中）](https://kaochenlong.com/2019/11/22/webpacker-with-rails-part-2/)
+ 
+ [Rails使用 Webpacker 管理靜態檔案](https://blog.wells.tw/posts/Rails%E4%BD%BF%E7%94%A8Webpacker%E7%AE%A1%E7%90%86%E9%9D%9C%E6%85%8B%E6%AA%94%E6%A1%88/)
+ [Rails使用Assets Pipeline管理靜態檔案](https://blog.wells.tw/posts/Rails%E4%BD%BF%E7%94%A8Assets-Pipeline%E7%AE%A1%E7%90%86%E9%9D%9C%E6%85%8B%E6%AA%94%E6%A1%88/)

