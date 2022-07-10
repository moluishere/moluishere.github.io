---
title: "【JavaScript】Cookie、LocalStorage、SessionStorage"
date: 2022-05-02
categories:
  - blog
tags:
  - HTTP
  - Cookie
  - LocalStorage
  - SessionStorage
---

關於瀏覽器儲存空間：Cookie、LocalStorage、SessionStorage

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/1*pv_Izc6m-aHltyZgLQqptQ.jpeg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">photo by @maxcodes on Unsplash</div>
</center>

> 前提：初學者的學習筆記，僅供參考，敬請指教～

## HTTP 超文本傳輸協定

在認識 `cookie`、`local Storage`、`Session Storage` 這些瀏覽器儲存空間之前，有一個很重要的觀念要先釐清：

> **HTTP 是沒有狀態的，它不會保存任何資料。**

先來看 [MDN](https://developer.mozilla.org/zh-TW/docs/Web/HTTP) 的介紹：

> _HTTP 遵循標準客戶端—伺服器模式，由客戶端連線以發送請求，然後等待接收回應。**HTTP 是一種無狀態協定，意思是伺服器不會保存任兩個請求間的任何資料 (狀態)**。_

這是一個很重要的前提，由於伺服器不會保存兩個請求之間的資料（狀態），如果沒有讓瀏覽器記住前端的的行為，只要重新整理頁面，我們所有在前端的操作就都會消失。

我們在日常的網路應用相當依靠這些瀏覽器儲存空間，例如：保留加入購物車的商品、網站記住帳號密碼、喜好（樣式）設定等等。

### 兩個請求端常用的代名詞

- 服務端（Server）/ 伺服器 / 後端
- 客戶端（Clint） / 瀏覽器（Browser）/ 前端

## Cookie

Cookie 是個小型文字檔，只有 4K 的儲存空間。是由 Server 發送給瀏覽器儲存，可設定儲存時效，瀏覽器在造訪網站時，便會帶著 cookie 提供 Server 辨認。

### Cookie 的原理

Server 在接收瀏覽器發送的 HTTP Request 時，會回傳帶有 `Set-Cookie` 欄位的 Response Header，並以鍵值對（key-value）的形式儲存：

```javascript
// 注意：vale 的型態必須是字串
Set-Cookie: <cookie-name>=<cookie-value>
```

**Cookie 就像一張號碼牌**，當瀏覽器接受後，會將 Cookie 的名稱（key）與值（value）儲存在瀏覽器的 Cookie 存放區，並記錄該 Cookie 隸屬的網域（domaim）、網址路徑、過期時間、是否為安全連線等資訊。

**之後每一次瀏覽器發送 HTTP Request 造訪該網站時，都會帶著 Cookie 。Server 透過這張號碼牌進行對比，以此識別不同的用戶。**

然而，由於 Cookie 容量小，並且會隨著每一次的 HTTP Request 傳輸，進而影響效能。 所以若不是需要記錄在 Server 的資料，可以使用 Web Storage 儲存。

Cookie 適用的情境有：廣告追蹤、身份驗證、購物車等。

### Cookie 與 Session

由於 Cookie 的特性（ 隨著每次的 HTTP Request 打到後端 ），以及限制（容量只有 4K ），Cookie 主要拿來儲存使用者 ID ，用來向後端進行身份驗證。

我們每做一次 HTTP Requset，瀏覽器就會拿 Cookie 去跟 Server 的 Session 做比對。 如果比對成功，Server 就會塞給 Cookie 一段 Token ，讓使用者拿這段 Token 去要資料。

**瀏覽器的 Cookie 與伺服器的 Session 可以說是成對存在的。**

## Web Storage

Web Storage 是 HTML5 提供在瀏覽器儲存資料的技術，並且有 **5MB** 的儲存空間。

和 Cookie 是經由 Server 提供給瀏覽器儲存不同，Web Storage 本身就只作用在瀏覽器。

Web Storage 分為 local Storage 與 Session Storage，兩者的差異在於生命週期的不同：

### Local Storage

生命週期：**永久存在**（除非手動刪除，否則會一直存在於瀏覽器）。

Local storage 和 cookie 一樣，都是以鍵值對(key-value)的形式儲存，value 也一樣必須是字串型態。

主要的使用方法就是 `setItem` 設定，並使用 `getItem` 讀取。

```javascript
//設定資料
localStorage.setItem(key, value);

//讀取資料
localStorage.getItem(key);

//刪除資料
localStorage.removeItem(key, value);
```

實務上更常使用 Local Storage，它適合拿來儲存不需要與 Server 溝通、較複雜且不敏感的資料，例如喜好設定、顏色樣式等等。

### Session Storage

生命週期：**暫存性質**（瀏覽器關閉及消失，不能分頁共享）。

Session Storage 的操作和 Local Storage 幾乎相同，只是由於生命週期的關係，通常拿來儲存更短期的資訊，例如有許多分頁的表單。

## 結論

- Cookie：容量小、可設定時效、會隨 HTTP 打到後端，適合拿來進行身份驗證（儲存 ID）。
- LocalStorage：容量較大、永久性的儲存空間，適合拿來儲存不需要與 Server 溝通、較複雜且不敏感的資料，例如喜好設定、顏色樣式。
- LocalStorage：容量較大、暫存性的儲存空間，適合拿來儲存短期的資訊。

參考資料：

- [Web Storage API（MDN）](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- [HTTP cookies（MDN）](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Cookies)
- [從 SessionStorage 開始一場 spec 之旅](https://blog.huli.tw/2020/09/05/session-storage-and-html-spec-and-noopener/)
- [[JavaScript] Cookie、LocalStorage、SessionStorage 差異](https://medium.com/@bebebobohaha/cookie-localstorage-sessionstorage-%E5%B7%AE%E7%95%B0-9e1d5df3dd7f)
- [HTML 5---[ API：Web Storage 瀏覽器儲存 ]---無用小觀念](https://ithelp.ithome.com.tw/articles/10187264)
- [[第七週] 瀏覽器資料儲存 - Cookie、LocalStorage、SessionStorage](<[https:/](https://yakimhsu.com/project/project_w7_storage.html)/>)
- [[第八週]網頁資料儲存 — cookie、local Storage、Session Storage](https://miahsuwork.medium.com/%E7%AC%AC%E5%85%AB%E9%80%B1-%E7%B6%B2%E9%A0%81%E8%B3%87%E6%96%99%E5%84%B2%E5%AD%98-cookie-local-storage-session-storage-a3f40013da37)
