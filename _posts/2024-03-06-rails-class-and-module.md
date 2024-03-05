---
title: "【Ruby】來點 Rails 吧 - class vs module"
date: 2024-03-04
categories:
  - blog
tags:
  - Rails
  - Class
  - Module
---

## 前言

在 Ruby on Rails 中，Class 和 Module 在實作上寫起來很相似。

那到底什麼時候該用 Class、什麼時候該用 Module 呢？

先從一個情境來下手吧。

## 情境 - 當我們要在專案中擴充功能


先來假設一個情境，我們在一個專案中要擴充功能，這個功能很簡單，我想要丟進一個數字，並且讓這個數字加一，這個功能可以寫成一個簡單的方法：

```ruby=
def add_one(number)
  number + 1
end
```

現在我有了 `add_one` 這個方法，那我要如何在專案中的任何地方都可以使用這個方法呢？

1. 寫一個 class，任何地方都可以呼叫：
 ```ruby=
 class Add
  class << self
    def add_one(number)
      number + 1
    end
  end
end
# Add.add_one(unmber) 呼叫
 ```
2. 寫一個 module，在要呼叫的地方 include/extend 這個 module：
 ```ruby=
 module Add
  def add_one(number)
    number + 1
  end
 end
 # include Add / extend Add
 # Add.add_one(number) / self.add_one(number) 呼叫
 ```

我們可以看到，在實作上寫 Class 或是 Module 都可以達到想要的效果，只是呼叫的方式不同。那我們在實作時，該如何選擇是要使用 Class 或是 Codule 呢？


## 定義 - Class vs Module

其實我們在初學 Ruby on Rails 時，都會學到這兩者的差別，讓我們來複習一下。先來看看 ruby doc 怎麼說的：

>Classes in Ruby are first-class objects—each is an instance of class Class. -- [ruby-doc](https://ruby-doc.org/3.2.2/Class.html)

>A Module in Ruby is a collection of methods and constants. --- [ruby-doc](https://ruby-doc.org/3.2.2/Module.html)

從這兩段內容，我們可以看到最明顯的區別 —— Class 是關乎物件（此處即指 instance）的；而 Module 則是關乎方法/常數集合的。

我們可將重點擺在最核心的差異 —— Class 具備實體化（new instance）的能力，而 Module 則否：

+ `Class` - 可以實體化（new instance），也就是可以創建物件（object）。
+ `Module` - 不能實體化（沒有 instance or object），它只是一個容器，用於封裝方法。


通常我們也會一併學習到「Class 有繼承能力，Module 則沒有繼承能力」。

這個結論，其實就是基於是否可以實體化。由於 Module 並不能產生實體／物件，自然也不會有繼承的需要。


## 結論

Class 和 Module 都讓我們能夠擴充功能，關於到底要使用 Class 還是 Module ，我們可以思考以下幾點：我們是否需要建立實體（instance）、擴充的功能跟現有的類別是否具有階層關係，以及擴充的功能跟現有的程式碼如何互動。

如果現在是在一個定義好的相關的 Class 裡，需要一個共用的方法，我們可以可以選擇使用 Class，將方法寫在父層，讓這個 Class 下面的實體都可以使用這個方法。

如果我們現在擴充的功能，是想要在整個程式碼的許多不相關的 Class 裡共用，我們就可以使用 Module，當我們需要時在 include/extend 即可。

參考文章：
+ [Difference between a class and a module](https://stackoverflow.com/questions/151505/difference-between-a-class-and-a-module/9778021#9778021)
+ [ Class Inheritance VS Modules in Ruby
](https://dev.to/abbiecoghlan/class-inheritance-vs-modules-in-ruby-1fha)---
