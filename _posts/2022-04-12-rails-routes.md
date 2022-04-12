---
title: "【Ruby】每天一點 Rails：route 建置 -- day 02 "
date: 2022-04-12
categories:
  - blog
tags:
  - Rails
  - route
---

介紹 Rails 中的 route 建置

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://railsbook.tw/images/chapter10/mvc.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">photo by 5xruby on 為你自己學 Ruby on Rails</div>
</center>

> 前提：初學者的學習筆記，僅供參考，敬請指教～

## Rails 裡的 route 與 MVC 運作

上一篇文章提到 Rails 是基於 MVC 架構開發，MVC 中的 Controller 掌控了資料流向與頁面邏輯，那 Controller 怎麼知道要處理哪個「頁面」呢？其實就是經由 route 的定義與指引。

## 什麼是 route？

route - 中文翻譯成「路由」，其涵義為路線、路徑。
它並不是網址（URL）本身，而是定義「網址」的「連線方式」以及「行為」的組合。

簡單來說，route 會定義：

**1. 該去哪裡（網址）**

**2. 去的方法（連線方式）**

**3. （使用者滿足條件後）指引到相應的控制中心（Controller & action）**

我們可以透過 route 定義網址、連線方式以及相應的 Controller；當使用者的 Request 滿足我們設定的條件時，網頁就會執行 Controller 設定的行為流程。

## 在 Rails 中定義 route

在 Rails 中路由都在 `routes.rb` 定義。我們可以靠指定的方式建立路由，或是使用「資源路由」（resource routing）一次產生出符合 RESTful 設計風格的路由。

> 關於 RESTful 會在之後的文章介紹。

### 指定路由

- 自行定義路由，指定網址與連線方式，並引導到特定 controller 中的 action。

先來看看下方的範例：

```ruby
get "/users", to: "users#index"
```

1. 網址："/users"
2. 連線方式：get
3. 控制中心：到 users 的 Controller，並執行 index 的 action 內容

使用者會透過 get 的方式打到 "/users" 這個位置，當使用者滿足條件，即執行 Controller 設計的流程：

```ruby
class UsersController <  ApplicationController
  def index
  # 滿足條件後，會執行此處的內容
  end
end
```

### 資源路由 resource routing

Rails 提供了一個符合 RESTful 風格的路由產生器，只要下一條指令就可以產生 8 條路徑與 7 個 action，我們先在 routes.rb 寫下這段指令 ：

```ruby
resources :users
```

在終端機我們可以使用 `rails routes -c users` 這個指令檢查 users 路由的狀態，產生的內容應該如下：

| HTTP Verb |          / 路徑           | controller action |
| :-------: | :-----------------------: | :---------------: |
|    GET    |     /users(.:format)      |    users#index    |
|   POST    |     /users(.:format)      |   users#create    |
|    GET    |   /users/new(.:format)    |     users#new     |
|    GET    | /users/:id/edit(.:format) |    users#edit     |
|    GET    |   /users/:id(.:format)    |    users#show     |
|   PATCH   |   /users/:id(.:format)    |   users#update    |
|    PUT    |   /users/:id(.:format)    |   users#update    |
|  DELETE   |   /users/:id(.:format)    |   users#destroy   |

資源路由提供了我們一套可以進行基本 CRUD 的路徑與 Controller action，我們就不用自己一個一個去定義。但遇到專案比較大的時候，我們還是得自行定義路由。

## 為什麼要用路由？

最後我們來看 Rails 官方手冊怎麼介紹路由的：

> The Rails router recognizes URLs and dispatches them to a controller's action, or to a Rack application. It can also generate paths and URLs, avoiding the need to hardcode strings in your views.

官方文件中，將 Rails 中的路由管理機制稱為 router （路由器）。
以本篇文章的討論內容來解讀該段落，擷取與本文相關的部分來說明，即「路由器可以辨識網址，並把網址分派至 Controller 進行處理，同時也可以建立路徑與網址。」

本篇內容些先到這裡，關於 route 的更多詳細內容，請大家至 [Rails Guide](https://guides.rubyonrails.org/routing.html) 查看吧！

參考資料：

- [Rails Routing from the Outside In -Rails Guide](https://guides.rubyonrails.org/routing.html)
- [Day21 route 路由（johnliutw）](https://ithelp.ithome.com.tw/articles/10207920)
