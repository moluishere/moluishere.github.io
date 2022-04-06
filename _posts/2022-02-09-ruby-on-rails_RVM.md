---
title: "【Ruby】學習 Ruby，從環境安裝開始！"
date: 2022-02-09
categories:
  - blog
tags:
  - Ruby
  - RVM
---

給新手以及 Windows 使用者的 Ruby 安裝指南

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

對於新手而言 — — 此處指的是毫無相關經驗與概念的超級新手，在剛開始接觸程式語言時，常常在基本概念上就卡住了。比如程式語言應該要「寫」在哪裡？程式要怎樣才會「動」？編輯器是什麼？語言還有分編譯器與直譯器？前端跟後端差在哪裡？諸如此類的基本概念，對於新手而言一開始都要花不少時間理解。

然而，以上這些概念查查資料都還能摸個大概，筆者認為最難跨越的就是「環境安裝」。由於現在有很多方便的線上編輯器，筆者也常常使用線上編輯器練習語法。但是，線上編輯器終究只能拿來練習「語法」，要進入程式開發的階段，我們還是必須要在電腦上進行，長痛不如短痛，就來認識一下 Ruby 安裝的步驟吧。

由於筆者使用的是 Windows 系統，本篇文章是針對 Windows 系統安裝 Ruby 做簡單的介紹，流程簡要說明如下：

> 1.  安裝 WSL（讓 Windows 可以執行 Linux 環境）
> 2.  安裝 Ubuntu （Linux 作業系統）
> 3.  使用 Ubuntu 下載 RVM（Ruby 版本控制器）
> 4.  使用 RVM 安裝 Ruby

## 前言：什麼是環境安裝？

一開始看到「環境安裝」，並不是很能直覺理解字面的意思。在多方查詢資料後，我個人的理解是：環境安裝就是指當我們要安裝一個軟體／程式語言、並預期它能順利執行時，電腦在作業系統、硬體、軟體各方面的最低條件。

就比如要煮一道菜，除了食材外還會需要鍋子與瓦斯爐。如果環境沒有配置好的話，程式語言是跑不動的，就像食材沒有適合的容器與溫度，也不能變成一道菜餚。

每個程式語言需要的安裝環境都不同，最保險的辦法就是去參考官方網站與文件：

> 根據 Ruby 的[官方網站](https://www.ruby-lang.org/zh_tw/downloads/)說明，每個主要的平台都有多種工具可安裝 Ruby：
>
> Linux/UNIX 平台，可以使用第三方工具（如 [rbenv](https://github.com/rbenv/rbenv) 或 [RVM](https://rvm.io/)）或使用系統套件管理工具；
>
> macOS 平台，可以使用第三方工具（如 rbenv 或 RVM）；
>
> Windows 平台，可以使用 [RubyInstaller](https://rubyinstaller.org/)。

簡單來說，就是依照不同的作業系統去下載相應的安裝程序，並使用這些程序來安裝 Ruby 的相關開發工具，以及支持環境的運行，這些下載的檔案通常也包含幫助文件等等。

例如我的電腦是 Windows 系統，官方建議的方法是使用 RubyInstaller，也就是說透過 RubyInstalle 在 Windows 的環境下去安裝並執行 Ruby 。

但若我們在網路上搜尋 Ruby 的相關資源，會發現大部分都是面向 Linux 或是 macOS 。原因是 Ruby 與 Linux / maOSc 系統的相容性更好，這與當初 Ruby 的開發環境有關。而 Ruby 在 Windows 環境下執行可能會產生很多意想不到的 bug ，所以還是不推薦直接在 Windows 下執行。

但使用 Windows 系統的人不用驚慌，也不用為此跑去買一台 Mac 。我們可以使用「Windows Subsystem for Linux」（[適用於 Linux 的 Windows 子系統](https://docs.microsoft.com/zh-tw/windows/wsl/faq)，簡稱：WSL） 來執行 Linux 。 WSL 是由微軟開發的一套與 Linux 相容的核心介面，可以在 Windows 環境中執行 Linux 。

而且現役的工程師總會告訴你「工程師怎麼可以不會 Linux ！」，所以我們一樣遲早是要學 Linux 的，我也才剛開始接觸，一起加油吧。

## 一、安裝 WSL（讓 Windows 可以執行 Linux）

那麼接下來就先介紹如何安裝 WSL，只需要兩個步驟，非常簡單：

- 於右下角搜尋欄輸入「PowerShell」，點按右鍵選擇「以系統管理員身分執行」，打開後貼上指令如下：

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linu
```

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*0pysG8HZUyj5Vko8">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">Windows PowerShell 應用程式／來源：Yvonne Shih</div>
</center>

- 重新開機 — — 沒錯，這樣就完成 WSL 的安裝了，接下來就來安裝 Ubuntu。

---

## 二、安裝 Ubuntu （ Linux 作業系統）

在這邊選擇的 Linux 作業系統是 Ubuntu。它是市面上使用最廣泛的 Linux 作業系統之一，具有教學資源多、美觀、指令簡易等特性，相當適合新手使用。Ubuntu 有內建打包好很多開源軟體，也可以使用它來下載安裝其它的軟體，這裡主要就是做一個安裝的動作，幫助我們等等下載 RVM。

在安裝好 WSL 並且重新開機後，打開 Microsoft Store，搜尋 Ubuntu 並進行安裝，總而言之新手就是先下載最新版本就對了（我也不會挑就是了 QQ）。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*zM1TbxtxA12qxgx9">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">Microsoft Store 中的 Ubuntu／來源：Yvonne Shih</div>
</center>

---

## 三、安裝 RVM （Ruby 版本控制器）

RVM 的全名是 Ruby Version Manage，簡單來說就是 Ruby 的版本控制器，提供 Ruby 不同的版本的環境與切換，避免電腦安裝不同版本的 Ruby 時發生衝突。

我們先打開剛剛下載的 Ubuntu，接下來的安裝的行為都在 Ubuntu 上進行：

- 進入 RVM 官網：[https://rvm.io/](https://rvm.io/)
- 複製首頁第二行（Installing RVM）的代碼：

```
  \curl -sSL https://get.rvm.io | bash -s stable
```

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*zM1TbxtxA12qxgx9">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">RVM 官網首頁／來源：Yvonne Shih</div>
</center>

- 在 Ubuntu 中貼上該行代碼並且執行。

- 此時有可能會出現缺少密鑰（key）導致連線失敗，可複製訊息中的 gpg 密鑰並且執行。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*btaHukGnGQ6oybmY">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">來源：Day 02 | 環境安裝 不可少的RVM（Aimee）</div>
</center>

- 也有可能再度報錯：gpg: keyserver receive failed: server indicated a failure. 此時依指示依序執行下方的兩行代碼，讓 RVM 的 Web 服務器導入密鑰。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*jXKSpeg0uHViLspH">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">來源：Day 02 | 環境安裝 不可少的RVM（Aimee）</div>
</center>

> 通常到這個步驟，應該就會在畫面最下方出現 donate 表示安裝完成，如果還有問題的話，請洽官網有關內容：[https://rvm.io/rvm/security](https://rvm.io/rvm/security)
>
> 上述的 gpg 密鑰 ，以及導入的代碼，都可以在該頁內容頁找到。

## 四、安裝 Ruby

安裝 Ruby 的各項指令也在 Ubunto 執行，在安裝好 RVM 後我們可以輕鬆使用各種指令來檢查、安裝、卸載 Ruby。

我們可以使用 rvm install 這個指令來安裝不同版本的 Ruby ，安裝好後再使用 rvm list 來檢查是否安裝好了，若不需要某個版本的 Ruby，則使用 rvm remove 即可卸載。

到這邊 Ruby 的環境配置與安裝就完成了喔！
RVM 相關指令如下：

> - rvm list known ：查看所有可安裝的 ruby 版本
> - rvm list：查看已安裝的版本
> - rvm install 3.0：安裝 Ruby 3.0（或任何你想要的版本）
> - rvm use 2.6.3：切換成 Ruby 2.6.3（或任何你已安裝的版本）
> - rvm remove 2.6.3：卸載 Ruby 2.6.3（或任何你已安裝的版本）

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*wh0H0G1vA_LUOz8T">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">rvm list known 指令畫面／來源：Yvonne Shih</div>
</center>

> 此篇文章主要是用學習筆記改寫而成，只是想要留個紀錄，並且希望能夠幫助到有需要的人，若內容有誤還請見諒與賜教～

參考資料：

- [Use windows or linux to start work with Ruby On Rails?](https://stackoverflow.com/questions/11648866/use-windows-or-linux-to-start-work-with-ruby-on-rails)
- [適用於 Linux 的 Windows 子系統常見問題集](https://docs.microsoft.com/zh-tw/windows/wsl/faq)
- [你認識最適合新手入門 Linux 發行版 Ubuntu 嗎？那你知道 Debian 為何常被一起提起？](https://progressbar.tw/posts/245)
- [環境設定\_為你自己學 Ruby on Rails（高見龍）](https://railsbook.tw/chapters/02-environment-setup)
- [Day 02 | 環境安裝 不可少的 RVM（Aimee）](https://ithelp.ithome.com.tw/articles/10216350)
