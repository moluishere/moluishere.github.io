---
title: "【Ruby】每天一點 Rails：Strong parameter -- day 03"
date: 2022-04-21
categories:
  - blog
tags:
  - Ruby
  - Rails
  - Strong parameter
---

關於 Rails 中的 Strong parameter

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

## Rails 當中的 params

Rails 中的 params 是我們從前端表單傳送進來的一大包資料。當我們在表單上點擊提交，會把一個 http request 傳送到後端伺服器，伺服器再根據請求回傳內容至瀏覽器。

在 Rails 這些 http request 會在 Controller 處理，後端會將傳送進來的內容轉變成可取用的變數 params ，例如使用者填寫進表單的內容，以及隨表單傳入 token，這包資料會以 hash 呈現。

而這些從前端傳進來的資料，會先經過 Strong parameter 的檢核才能存進資料庫。

## Strong parameter 是什麼？

Strong parameter 是 Rails 的一種防護機制，它的目的是防止有人透過竄改表單欄位，將不應該出現的資訊傳入資料庫。換句話說，使用者只能使用我們所設計且允許的表單欄位傳送資料，若有人在前端設計、更改欄位想要傳送資料時就會被阻擋下來。

我們來看看官方手冊關於 Strong parameter 的描述：

> It provides an interface for protecting attributes from end-user assignment. This makes Action Controller parameters forbidden to be used in Active Model **mass assignment** until they have been **explicitly enumerated**.

Rails 提供一個 interface 在終端使用者的操作下保護屬性（attributes），其藉由 Action Controller 來處理大量賦值（mass assignment），透過白名單來過濾不可賦值的參數，也就是明確指定那些屬性可以賦值。

那麼，大量賦值（mass assignment）又是什麼呢？

## 什麼是 mass assignment ?

大量賦值是 Rails 提供的的一個功能，範例如下：

```ruby
user = User.new
user.name = "Francesco"
user.email= "aa@bb.cc"
user.save
```

`new` 這個方法接受 hash 做賦值， 因此可以寫成：

```ruby
# {:name: "Foo", :email : "aa@bb.cc"} 的{}可省略
User.new( :name: "Foo", :email : "aa@bb.cc" )
```

這種一次性對實體化出來的物件進行多個 attributes 賦值，就稱為大量賦值。
最後我們呼叫 `save` 方法，就可以把資料存進資料庫了。

## Form 與 mass assignment

我們可以想像，通常我們設計的表單會有好幾個欄位請使用者 input 進去，在 Rails 中會是一包 params，「大量賦值」後「save」的行為實際上就是「將表單傳入的 params 存到資料庫」，這時如果那包 params 沒有做任何資料清洗，在我們存入資料庫時就會報錯：

```ruby
class UsersController < ApplicationController
  # 會拋出 ActiveModel::ForbiddenAttributes 異常。
  # 因為做了大量賦值卻沒有明確的說明允許賦值的參數有哪些。
  def create
    @user = User.new(params[:user])
    @user.save
  end
```

## Strong parameter 使用方法

前面我們說到，如果我們沒有對那包資料做任何清洗，畫面就會出現 `ActiveModel::ForbiddenAttributes`
的錯誤訊息。這時我們可以使用 Rails 提供的方法來設定白名單。

我們通常是使用 `require` 與 `permit` 兩種方法。

假設我們傳進來的 params 如下：

```ruby
user: { username: "Francesco", :email: "aa@bb.cc" }
```

我們可以透過 `require` 拿到 :username 這個 key，再透過 `permit` 允許 :username， :email 包含這兩個 key 本身的 hash 進來。

通常我們會使用 `private` 方法來封裝允許大量賦值的參數，這麼做的好處是這個方法可以在 Controller 重複使用：

```ruby
 private
    def claen_params
      params.require(:user).permit(:name, :age)
      # 也可以寫成 params.[:user].permit(:name, :age)，但我們通常都使用 require
    end
end
```

此時我們使用清洗過的資料當作參數，就可以順利存入：

```ruby
class UsersController < ApplicationController
  def create
    @user = User.new(claen_params)
    @user.save
  end
```

在 Rails 中，Strong parameter 的設計，讓我們必須確保資料經過清洗，以保護我們的資料庫。

參考資料：

- [Strong Parameters（Rails Guide）](https://edgeapi.rubyonrails.org/classes/ActionController/StrongParameters.html)
- [[ Rails guide study ] Day03 Strong parameters 介紹(Getting Started with Rails part2)](https://ithelp.ithome.com.tw/articles/10214372)
