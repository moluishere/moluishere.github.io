---
title: "【Ruby】每天一點 Rails：find()、find_by()、where()"
date: 2022-04-24
categories:
  - blog
tags:
  - Ruby
  - Rails
---

關於 Rails 中的 find()、find_by()、where()

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

## find()

> _預設只能使用 id 進行查詢，找出單一筆資料。_

1. 參數：要查找的對象的 ID。
2. 如果找到：返回查詢對象。
3. 如果沒找到：引發 `ActiveRecord::RecordNotFound` 例外訊息。

```ruby
def show
  @model = Model.find(params[:id])
  # @model = Model.find(1)
end
```

由於有例外訊息，我們就可以使用 `begin...rescue` 來捕捉：

```ruby
def show
  begin
    @model = Model.find(params[:id])
  rescue
    redirect_to show_path, notice: "沒有這個 id！"
  end
end
```

## find_by()

> _可設定條件進行查詢，找出符合條件的第一筆資料。_

1. 參數：物件（key : value）。
2. 如果找到：返回查詢對象。
3. 如果沒找到：返回 nil 。

```ruby
def show
  @model = Model.find_by(name: params[:name], age: params[:age])
  # @model = Model.find_by(name: "Bob", age:24)
end
```

如果僅使用 id 進行查詢，效果會和 `find()` 相同：

```ruby
def show
  @model = Model.find_by(id: params[:id])
  # 等同於：@model = Model.find(params[:id])
end
```

由於使用 `find_by()` 沒找到對象會返回 nil，若我們希望可以像使用 `find()` 時得到 `ActiveRecord::RecordNotFound`，我們可以在 find_by()後面加一個驚嘆號，就可以像`find()`捕捉例外訊息。

```ruby
def show
  @model = Model.find_by!(id: params[:id])
end
```

## where()

> _可設定條件進行查詢，並返回符合條件的所有資料。_

1. 參數：字串、陣列、物件（key : value）。
2. 如果找到：返回 `ActiveRecord::Relation` 包含一個或多個與參數符合的對象。
3. 如果沒找到：會返回一個空的 `ActiveRecord::Relation`。

> 【注意】條件可以是字串、陣列、或是 Hash。但純字串的條件會有　**SQL injection** 的風險，所以通常會改用陣列處理。

### 字串

直接將要使用的條件，以字串形式傳入 where 即可：

```ruby
def show
  @model = Model.where("name = 'bob'")
end
```

然而，如果我們的條件內有變數，字串條件就會有資料庫注入的風險 :

```ruby
Model.where("name = '%#{params[:name]}%'")
```

這種情況，應改為陣列方式處理：

```ruby
def show
  @model = Model.where("name= ?", params[:name])
end
```

Rails 會將 `?` 換成 `params[:name]` 做查詢。條件式後的元素，對應到條件裡的每個 `?`。

來看看[官方手冊](https://rails.ruby.tw/active_record_querying.html)的提醒：

> 直接將變數插入條件字串裡，不論變數是什麼，都會直接存到資料庫裡。這表示從惡意使用者傳來的變數，會直接存到資料庫。這麼做是把資料庫放在風險裡不管啊！一旦有人知道，可以隨意將任何字串插入資料庫裡，就可以做任何想做的事。**絕對不要直接將變數插入條件字串裡**。

### 陣列

前面已經提到如何使用陣列進行查詢：

```ruby
def show
  @model = Model.where("name= ?", "params[:name]")
end
```

`Were()` 可以設下多個條件，使用 AND（&） 連接：

```ruby
def show
  @model = Model.where("name= ?　AND　age ?", "params[:name]", "params[:age]")
end
```

上面第一個 `?` 會換成 `params[:name]`，第二個則會換成 `"params[:age]"` 。

### 物件

若使用 `key:value` 做為查詢的參數，和 `find_by()` 的使用方式相同。差別在於 `find_by()` 只會返回符合條件的「第一筆資料」，where 則是會回傳符合條件的「全部資料」。

```ruby
def show
  @model = Model.find_by(name: params[:name], age: params[:age])
  # @model = Model.find_by(name: "Bob", age:24)
end
```

## 其他查詢方法

```ruby
Model.all　# 找出所有資料
Model.first # 找出第一筆資料
Model.last # 找出最後一筆資料
```

參考資料：

- [Active Record 查詢（Rails Guide）](https://rails.ruby.tw/active_record_querying.html)
- [Day24: Rails 中的 find? find_by? where?（Louis）](https://ithelp.ithome.com.tw/articles/10225325)
- [Ruby on rails - find-vs-find-by-vs-where（stack over flow）](https://stackoverflow.com/questions/11161663/find-vs-find-by-vs-where)
