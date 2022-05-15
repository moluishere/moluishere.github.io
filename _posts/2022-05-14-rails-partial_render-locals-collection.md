---
title: "【Ruby】每天一點 Rails：partial render：locals、collection"
date: 2022-05-14
categories:
  - blog
tags:
  - Rails
  - partial render
  - locals
  - collection
---

 關於 rails 中的 partial render

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

在 rails 裡，重複使用到的 view 可以進行模組化，使用的方法叫做 partial render（局部渲染）。

在進行 partial render 時可以傳遞變數進去，避免 view 在空中抓實體變數的問題，這又分為 `locals`、`collection` 兩種方法。

## partial render

partial render 可以理解為「借畫面來渲染」。當一段 view 重複被使用到時，我們就可以把它獨立整理一個檔案，讓需要這段 view 的頁面去渲染該檔案。

在進行局部渲染時，慣例是在渲染的檔案前加底線 `_`。比如 `index.html.erb` 去局部渲染 `"form"`，而這個檔案的名稱會是`_form.html.erb`：
```html
# index.html.erb

<h1>這是 index 的內容</h1>
<%= render partial: "form" %>
```
`<%= render partial: "form" %>` 也可以簡寫成：
```html
<%= render "form" %>
```
渲染檔名需要加底線 `_`：
```html
# _form.html.erb

<h2>這是 form 的內容</h2>
```
![](https://miro.medium.com/max/1208/0*_a0zi5oNcu50LN73.png)


## locals

通常我們在渲染的時候，會搭配從資料庫的撈出來的資料進行渲染。
我們在 controller 處理時會是一個實體變數，再讓相應的 view 取用。


locals 可以讓實體變數轉變成區域變數，使得局部渲染的檔案被動接受資料，而不需要去抓空中的實體變數。

這回應到 MVC 的應用原則，view 應該只負責呈現畫面，而不會有太多主動的行為。

以下為 view 抓取實體變數的例子（不推薦）：

```html
# index.html.erb

<h1>這是 index 的內容</h1>
<%= render "form" %>
```

```html
# _form.html.erb

<h2>這是 form 的內容</h2>
<div> <%= @users %> </div>
# 要主動去找有沒有 @user 這個實體變數
```
可以使用 locals 改成：
```html
# index.html.erb

<h1>這是 index 的內容</h1>
<%= render partial: "form", locals:{users: @users} %>
# 將實體變數 @user 轉為區域變數 user ，傳進局部渲染的檔案
```
```html
# _form.html.erb

<h2>這是 form 的內容</h2>
<div> <%= users %> </div>
# 被動接受區域變數
```
`<%= render partial: "form", locals: {users: @users} %>`可以簡寫成：
```html
<%= render "form", users: @users %>
```
這個寫法
## collection
collection 除了可以像 locals 做到將實體變數轉為區域變數，還順便幫我們把 each 迴圈做完了。不過要使用 collection 需要依賴相當多慣例，先舉一個例子：

```html
# index.html.erb

<h1>這是 index 的內容</h1>
<%= render partial: "user", collection:  @users} %>
```

```html
# _user.html.erb

<h2>這是 user 的內容</h2>
<div> <%= user.name %> </div>
# 不用再做迴圈，就可以把東西一個一個印出來
```
使用 collection 需要許多的巧合：

1. 局部渲染的檔名（`_user.html.erb`）必須以那包資料(`@users`)的「單數」命名。
2. 局部渲染的檔案必須放在同個資料夾層級。
3. 局部渲染檔案內的區域變數必須是單數（`user`）。 

而且`<%= render partial: "user", collection:  @users} %>`還可以簡寫成：
```html
<%= render @users} %>
```
假如可以把這些巧合湊齊，就能使用這短短的程式碼進行 collection 渲染了。

參考文章：
+ [Layouts and Rendering in Rails(Rails Guide)](https://guides.rubyonrails.org/layouts_and_rendering.html)
+ [ [ Rails guide study ] Day11 教你怎麼傳變數給 partial](https://ithelp.ithome.com.tw/articles/10216293)

