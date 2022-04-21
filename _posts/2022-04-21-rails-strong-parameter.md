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

## Strong parameter 是什麼？

Strong parameter 是 Rails 的一種防護機制，它的目的防止有人透過竄改表單欄位將不應該出現的資訊傳入資料庫。換句話說，使用者只能使用我們所設計且允許的表單欄位傳送資料，若有人在前端設計其他欄位想要傳送資料時就會被阻擋下來。

我們來看看官方手冊關於 Strong parameter 的描述：

> It provides an interface for protecting attributes from end-user assignment. This makes Action Controller parameters forbidden to be used in Active Model **mass assignment** until they have been **explicitly enumerated**.

Rails 提供一個介面 Action Controller 來處理大量賦值（mass assignment），透過白名單來過濾不可賦值的參數，也就是明確指定那些屬性可以賦值。

我們可以想像「大量賦值」指的就是「透過表單寫入 params 到資料庫」，這時如果那包 params 沒有做任何資料清洗，在我們存入資料庫時就會報錯：

```ruby
class PeopleController < ActionController::Base
  # 會拋出 ActiveModel::ForbiddenAttributes 異常。
  # 因為做了大量賦值卻沒有明確的說明允許賦值的參數有哪些。
  def create
    Person.create(params[:person])
  end
```

## Strong parameter 使用方法

前面我們說到，如果我們沒有對那包資料做任何清洗，畫面就會出現 `ActiveModel::ForbiddenAttributes`
的錯誤訊息。這時我們使用可以使 Rails 提供的方法來設定白名單。

我們通常是使用 requier 與 permit 兩種方法。

假設我們傳進來的 params 如下：

```ruby
# 隨表單傳入的資料應該還有從表單生成的 token，此處先不列出。

person: { name: "Francesco", :age: 25 }

```

我們可以透過 require 拿到 :person 這個 key，再透過 permit 允許 :name， :age 包含這兩個 key 本身的 hash 進來：

```ruby
 private
    # 使用 private 方法來封裝允許大量賦值的參數
    # 這麼做的好處是這個方法可以在 Controller 重複使用。

    def person_params
      params.require(:person).permit(:name, :age)
    end
  # 也可以寫成 params.[:person].permit(:name, :age)，但我們通常都使用 require

end
```

```

參考資料：

- [Strong Parameters（Rails Guide）](https://edgeapi.rubyonrails.org/classes/ActionController/StrongParameters.html)
- [[ Rails guide study ] Day03 Strong parameters 介紹(Getting Started with Rails part2)](https://ithelp.ithome.com.tw/articles/10214372)
```
