---
title: "【GitHub】搶救綠格子"
date: 2022-07-31
categories:
  - blog
tags:
  - GitHub
  - Git
  - GitHub contributions
---

關於 Github 中的 contributions

> 前提：初學者的學習筆記，僅供參考，敬請指教～

有天，我看著 GitHub 的個人頁，發現有點奇怪：

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/1400/0*Bvv-vUBqVkJUdUMg.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px"></div>
</center>

這個綠色格子分佈不合理！

```
這些綠色的格子，是我們每次給 GitHub 提交程式碼可以獲得的貢獻紀錄，游標移到格子上面，顯示的是當日有幾次提交的 commit。顏色深淺度是按照 commit 數的比例分配，顏色越深代表越多 commit 。
```

專案開始時大約是四月底，五、六月時每天都在寫 code，而且都有 push 到 GitHub 上。但綠色格子的的 commit 數不僅很低，而且有些天還是空白的，跟實際狀況差滿多的。於是我決定要搶救我的綠色格子！

## 綠色格子（提交貢獻）的條件

[GitHub](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-settings-on-your-profile/why-are-my-contributions-not-showing-up-on-my-profile) 官方有說明達成綠色格子的條件，可以根據說明逐項檢查。經過排查，我發現我的狀況是 GitHub 上的 email 和我 Git 設定的 user email 不一樣。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/1400/0*tzFYAnKMEaLzxfjG.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px"></div>
</center>

原因是我中途換過電腦，而我並沒有重新設定 Git 的 user email，導致我推上去的內容，沒有被 GitHub 認定。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/1400/0*rjs5duUZiz7sQRu5.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">上方是原來的設定，下方是 Git 默認電腦用戶</div>
</center>

## 檢查 Email 設定並修改

首先，我們可以檢查我們 GitHub 上的 email 和 Git 設定的 user email 是否一致，不同的話就修改成一樣的 email。

### GitHub Emails

點擊個人頭像進入 settings，再進去 Emails 確認：

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/1400/0*eaMoV-qdJ28XzSHe.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">GitHub settings sidebar 截圖</div>
</center>

如果我們這裡設定的 email 是 `aa@bb.cc` ，記得 Git 的 user email 就要設定成 `aa@bb.cc` 。

### Git user email

確認好 GitHub 上的 email 設定後，接著就打開終端機檢查 Git 的 user email，也順便檢查 user name 的設定吧：

```
# 檢查 user email 設定
git config --global user.email

# 撿查 user name 設定
git config --global user.name
```

接著來修改設定：

```
# 修改 user email 設定
git config --global user.email <your user email>

# 修改 user name 設定
git config --global user.name <your user name >
```

注意，不管是 email 還是 name，都不需要引號 ""。
程式碼看起來會像是：

```
git config --global user.email aa@bb.cc

git config --global user.name apple
```

如此一來，往後我們的 commit 就會被紀錄。

## 搶救綠色格子

接下來就開始搶救過往的綠色格子，也就是修改 commit 的紀錄，把 email 改成正確的設定。

我們可以使用 `--env-filter` 去過濾 commit 並且修改：

```
git filter-branch --env-filter '
    if test "$GIT_AUTHOR_EMAIL" = "old email"
    then
        GIT_AUTHOR_EMAIL=aa@bb.cc
    fi
    if test "$GIT_COMMITTER_EMAIL" = "old email"
    then
        GIT_COMMITTER_EMAIL=aa@bb.cc
    fi
' -- --all
```

通常作者（author）大部分都等同於提交者（committer），當然專案中會有作者跟提交者不一樣的狀況，我們就一起更改吧！

[2.3 Git 基礎 - 檢視提交的歷史記錄](https://git-scm.com/book/zh-tw/v2/Git-%E5%9F%BA%E7%A4%8E-%E6%AA%A2%E8%A6%96%E6%8F%90%E4%BA%A4%E7%9A%84%E6%AD%B7%E5%8F%B2%E8%A8%98%E9%8C%84#rlog_options)：

> _你可能會好奇「作者（author）」與「提交者（committer）」之間的差別， 作者是最初修改的人，而提交者則是最後套用該工作成果的人； 因此，如果你送出某個專案的補綴，而該專案其中一個核心成員套用該補綴，則你與該成員都有功勞——你是作者，而該成員則是提交者。_

修改好後，我的綠色格子終於回復拉！

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/1400/0*2vyykEj4P8dNYyCd.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px"></div>
</center>

參考文章：

參考文章：

- [How do I change the author and committer name/email for multiple commits?
  ](https://stackoverflow.com/questions/750172/how-do-i-change-the-author-and-committer-name-email-for-multiple-commits)(stackoverflow)

- [2.3 Git 基礎 - 檢視提交的歷史記錄](https://git-scm.com/book/zh-tw/v2/Git-%E5%9F%BA%E7%A4%8E-%E6%AA%A2%E8%A6%96%E6%8F%90%E4%BA%A4%E7%9A%84%E6%AD%B7%E5%8F%B2%E8%A8%98%E9%8C%84#rlog_options)（Git）
- [修复 git 提交不显示 commit 贡献小绿点【git bash】](https://codeantenna.com/a/ak27IiJb0s)(CodeAntenna)
