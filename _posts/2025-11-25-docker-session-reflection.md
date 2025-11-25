---
title: "小賴老師「Docker 實戰工作坊」心得分享"
date: 2025-11-25
categories:
  - blog
tags:
  - Docker
---
這篇是純心得與推廣文。

先簡單介紹一下背景，我是轉職三年多的後端工程師，非本科。

---

## 報名動機

今年九月初我報名了小賴老師的 [Docker 實戰指南工作坊](https://5xcampus.com/courses/docker.html)。起因是同事揪團，動念是想加強自己對 docker 的理解。

我所在的工作團隊，已從去年即逐步將開發與正式環境容器化，透過的正是 docker 這個工具。這部分的推進是幾位前輩推進的，開發環境還安裝了 [*Lazydocker*](https://github.com/jesseduffield/lazydocker) ，指令也不太需要自己打。

在推進的過程中，為了不落太多知識點，當時有去 docker 官方 [Introduction](https://docs.docker.com/get-started/introduction/) 看了一輪，並跟著實作，讓自己至少知道基本概念與指令，也有在 YT 上找一些課程來看。但開發中也比較常透過 lazydocker 在操作 。又因為是 container，所以出了問題，總之砍掉重啟就是（然後切記不能砍  volume ）。

思量再三，最後跟同事一起報名了這堂課，經過兩個下午的洗禮，我對 docker 有了更深幾層的認識，課程真的物超所值！

---

## 不要只會用，要你真的懂

小賴老師從容器技術開始講起，沒錯就是從「Docker 是基於 Linux 核心功能實踐的技術」開始。container 的核心能力來自 Linux Kernel（namespaces、cgroups、UnionFS），docker 不是容器本體，而是用來管理容器的工具與生態系。

雖然是底層的內容，但知道這些，才能真的了解容器技術為什麼能提供隔離的環境。也才能理解 docker 實際上做的事將容器技術標準化，進而變成業界標準。

小賴老師也有介紹 **process（行程）** 的概念，並藉由這個觀念來說明 container 為什麼能做到隔離。老師用非常好理解的方式，把 process、PID、namespace 的關係講得很清楚，讓我補上了過去一直欠缺的系統基礎知識。

筆記：

> Docker 基礎 = Linux process + namespace + cgroups + union filesystem
**⇒ container 其實就是一個被隔離起來的 process**
>

## 指令是基本，實作最重要

課程內容真的是全方位，指令只是最基本（當然也教得很仔細）。最讓我深刻的是老師上課時的實際操作、突襲我們的實作，以及課後的作業。

實作演練與課後作業的部分，我最印象深刻的就是現場 debug 了，真的很有趣，也真的讓我冷汗直流，彷彿在上班。課後作業讓我們建立一個 WordPress + MySQL 的部落格、改寫 compose file 也非常有收穫。就不再多講了，以免大爆雷。

## 懂了以後，還要優化

在介紹完 docker 的技術基礎、什麼是 Dockerfile、 image、 container、docker network、docker storage 之餘，老師並不僅止於教學，還有介紹該如何優化。Dockerfile 是用來 build image，由於 docker 的 layer 架構，順序會有差別，且前面的 layer 有 cache，後面也能沿用。基於這些特性，有這些點可以注意，以達到 image 更小、build 更快、可維護度更高的目的。簡要筆記如下：

- 儘量使用官方的 image。
- 把變動少的步驟放前面，把變動多的步驟放後面（for cache）。
- 每個 run 都是一個 layer，所以清理檔案要在同一個 RUN 指令中完成。

這些重點，老師也是一行一行 run 給我們看，因此對我而言並不只是文字上的吸收，而是看到優化前後的差異，讓人非常深刻，不會過目即忘。

## 技術真有趣

小賴老師在課堂上示範了幾個技術探討的操作，真的很有趣。

比如介紹 layer 時，提到 image layer 是 ready-only，所以如果是從 image layer 來的檔案，在 container 刪除，實際上 docker 是用另一個檔案覆蓋，在 host 是可以查到那個覆蓋檔。所以如果刪除那個覆蓋檔，container 的檔案又會出現。

又比如，上面提到的行程，老師也示範在 host kill 掉 container 跑起來的的 process（ex: 在 host 環境中，PID: 532879），還是可以在 host 查到這個 process，但這個 process 實際上已經死了。而 container 的父行程（PID: 1）還在跑，讓 host 的子行程（PID: 532879）就會變成殭屍（沒有在跑，但佔一個 ID）。

當然一般來說不會做這樣的操作，是純技術探討，但真的讓課程變得很生動。

## 老師好用心

老師的用心真的要大推，兩次上課都講到超時，課後作業也很用心批改，同學的問題有問必答，而且回答得很詳盡，講到要讓同學務必聽懂。真的感受到老師充滿了教學的熱忱。

我覺得最棒的就是小賴老師準備了非常多的操作案例、實戰演練和作業。透過一直動手做、反覆練習，我是真的有把東西學進去。

## 課程收穫

我終於不是只會「用」，而是多少知道一些原理。

比如我終於知道為什麼 docker 需要特別處理 storage 了。因為 container 裡的所有變動，都只會寫在可變層（container layer）上，生命週期是跟著 container 走的。如果不做任何持久化設定（像是 volumes 或 bind mounts），所有在容器內新增、修改的檔案，會在 container 一關閉就完全消失。

我最一開始使用 docker 時，曾經不小心砍掉 volumn 而導致遺失我的更動。雖然我也有查詢資料，了解 docker 必須做 storage 的設定才能保存改變，但我其實是不知道原理的。經過這次課程，我才了解背後的原理。

還有像好用的指令、dockerfile 優化等等，我想之後一定用得上。

## 課程推薦

只要有想要了解 docker，我都真心推薦！

但要有點開發經驗為佳，不然課程其實滿硬的。
