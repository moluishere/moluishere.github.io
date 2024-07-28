---
title: "【Ruby 筆記】scope method vs class method "
date: 2024-07-28
categories:
  - blog
tags:
  - Rails
  - class mthod
  - scope method
---

在 ruby on rails 中，當我們進入一個 model class，我們可以看到以下 3 種方法：

1. class method
2. instance method
3. scope method

先來介紹這 3 種 method，最後再來聊聊 scope method vs class method


假設我們有一個 model `User`，以下分別說明 & 舉例：

### 1. Class Method

**定義**：顧名思義，這是定義在 model class 上的方法，跟 instance 無關。我們可以直接在 class 上呼叫這個方法。

**ex**：

>+ User 身上有個欄位 `active`，`type` 為 `boolean`，代表這個 user 是否啟用。
>+ 我現在想要找到所有啟用的 user，條件為  `active` 為 `true`。

```ruby
# 定義一個 class mehotd 來找到所有啟用的使用者
class User < ApplicationRecord
  def self.active_users
    where(active: true)
  end
end
```

**call**：

```ruby
# 透過呼叫 actice_users 來獲得所有啟用的使用者
User.active_users
```

在這個例子中，`active_users` 是一個 class 方法，它會返回所有 `active` 欄位為 `true` 的 user。

### 2. Instance Method

**定義**：這是定義在 model instance 上的方法，必須有一個具體的 instance 才能使用。

**ex**：
> + 定義一個 instance method 來返回 user 的全名，使用 user 身上的 `first_name` & `last_name` 欄位組成。

```ruby
class User < ApplicationRecord

  def full_name
    "#{first_name} #{last_name}"
  end
end
```

**call**：

```ruby
# 假設有一個 user instance
user = User.find(1)
# 呼叫 instance mthod 來得到該 user 的全名
user.full_name
```

在這個例子中，`full_name` 是一個 instance 方法，它會返回 user 的全名。我們需要有一個 `user`  instance 來使用這個方法。

### 3. Scope Method

**定義**：這是用來定義查詢條件的方法。我們會在 scope mehotd 中定義查詢條件，並且 scope method 永遠回傳 `ActiveRecord::Relation` object。

**ex**：

```ruby
class User < ApplicationRecord
  # 定義一個 scope mehotd 來找到所有啟用的使用者
  scope :active, -> { where(active: true) }
end
```

**使用方式**：

```ruby
# 透過呼叫 active 來獲得所有啟用的使用者
User.active
```

在這個例子中，`active` 是一個 scope 方法，它會返回所有 `active` 欄位為 `true` 的 user。

---

## scope method vs class method

看完上述所舉的例子，應該就可以發現 class method & scope method，如果用於`條件查詢`，效果看起來似乎一樣。

但 scope method 跟 class method 有個最大的差異：scope method 中只能定義查詢語句，並且永遠都會回傳 ActiveRecord::Relation object，即便它的條件為 false。



這是因為 scope 的目的是提供一個查詢條件，並且可以進行後續的串連使用。

**ex：**

```ruby
class User < ApplicationRecord
  # 定義一個 scope mehotd 來找到所有啟用的使用者
  # 目前所有 user 都還未啟用，active 欄位都是 false
  scope :active, -> { where(active: true) }
end

# 此時透過呼叫 active 來獲得所有啟用的使用者，但並沒有符合條件的 record
User.active
#=> []

# 如果我們在使用 class 檢查
User.active.class
#=> User::ActiveRecord_Relation < ActiveRecord::Relation
```

另一方面，我們也可以讓 class method 返回 ActiveRecord::Relation，讓其結果可以用於進一步的查詢。但我們也可以讓 class method 返回非 relation 的 object。

### use case

這樣我們可能會想，那何不都使用 class method 就好了呢？
但 scope method 也有其優勢。它看起來更加整潔、易讀，並且直接傳達了它作為查詢語句的意圖。

當我們在思考要使用 scope method or class method 的時候，可以考慮下述情境：

#### 使用 scope

當我們需要簡單的查詢條件，並希望支持串連使用。scope 適合用於那些查詢條件簡單、可以多次串連的情況。

#### 使用 class method

當查詢邏輯複雜、需要額外參數，或當查詢結果不符合 ActiveRecord::Relation 的預期（例如返回單一記錄或 nil）時。class method 提供了更多的靈活性和處理能力。

### Reference
+ [Scope vs Class Methods](https://dev.to/truptihosmani/scope-vs-class-methods-2jee)
+ [Should You Use Scopes or Class Methods?](https://www.justinweiss.com/articles/should-you-use-scopes-or-class-methods/)
+ [Active Record Query Interface - scope](https://guides.rubyonrails.org/active_record_querying.html#scopes)
