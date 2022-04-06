---
title: "【Javascript】印出九九乘法表 & 三角形－筆記欄目"
date: 2022-03-08
categories:
  - blog
tags:
  - javaScropt
  - Array
---

在每個程式語言的學習過程中，印出九九乘法表與三角形似乎都是一個經典題型。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*eVCEJzH6xLSof2ql.jpeg">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">photo by Gabriel Heinzer on Unsplash</div>
</center>

> 前提：初學者的學習筆記，僅供參考，敬請指教～

在每個程式語言的學習過程中，印出九九乘法表與三角形似乎都是一個經典題型。
分析這兩類題目的脈絡，目的應該是考驗「找出規律」的邏輯能力，以及找出規律後，使用程式語言知識來解題的能力。其實，一個題目可以有很多種解法，以下僅分享個人的心得與筆記。
在分析題目後，筆者發現這兩個題目的共通點：

1. 都具有重複執行某些特定的敘述。
2. 這些特定的敘述受到兩個或兩個以上的因素而改變，且同樣重複執行。

當出現「重複執行某些特定的敘述」時，就是使用迴圈的時候了。又這些特定的敘述改變時，其他敘述也跟著重複變化，這種時候就可以使用「巢狀迴圈」。

【重點】巢狀迴圈
簡單來說，其實就是迴圈包迴圈的結構，也就是在迴圈的區塊語句中，再加上另一個內部的迴圈語句。外部的迴圈每執行一次，內部的迴圈會全部跑完一次。

引用[Delft Stack](https://www.delftstack.com/zh-tw/howto/javascript/nested-for-loops-javascript/)上對於巢狀迴圈的說明：

> 巢狀迴圈是一種程式設計結構，用於迭代一系列資料或重複執行相同的操作，直到滿足特定條件或持續一定的時間，而無需一次又一次地明確編寫程式碼。
> 巢狀的 for 迴圈是迴圈的組成。我們可以在一個迴圈中存在一個或多個迴圈。巢狀的迴圈稱為內部迴圈，而包含巢狀迴圈的迴圈稱為外部迴圈。
> 執行總是從外迴圈開始，然後向下移動巢狀迴圈。內部迴圈在外部迴圈的每次迭代中完全執行。我們可以將巢狀迴圈的語法大致定義為：

```javascript
Outerloop{
	Innerloop{
	// statements to execute inside inner loop
	}
	// statements to execute inside outer loop
}
```

> 迴圈可以是任何型別，例如 for 迴圈，while 迴圈或 do-while 迴圈。

要注意的是，「巢狀迴圈」會先從最內層的迴圈開始執行，執行完畢後再執行外層的迴圈。

---

【題目 1】使用迴圈列印出九九乘法表
九九乘法表的概念，其實就是 1-9 的每個數字（被乘數），都會去乘上 1~9（乘數）。依照下面的程式碼，外層迴圈的 i 就是我們被乘數的 1~9，內層迴圈的 j 則是乘數的 1~9。
以下說明：外層迴圈第一次執行時，i = 1 ，外層迴圈為 true，外層迴圈會執行一次；此時內層迴圈也跟著執行，j = 1，內層迴圈為 true 並會完全執行，j 會跑出 1~9。如此一來，九九乘法表中的被乘數 1 與乘數 1~9 就出來啦。同理，外層迴圈進入第二次迴圈（i = 2）時，就會產生被乘數 2 ，內層迴圈（j = 2）再次完全執行，產生 1~ 9 的乘數…..以此類推：

```javascript
// 印出99乘法表
for (let i = 1; i <= 9; i++) {
  for (let j = 1; j <= 9; j++) {
    console.log(i + "*" + j + "=" + i * j);
  }
}
```

---

【題目 2】印出 5 階 直角三角形 & 印出 5 階 等腰直角三角形

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"
    src="https://miro.medium.com/max/875/0*ZBhki1K1pKuQfn9M">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">五倍紅寶石．程式教育機構</div>
</center>

【基本款】

有了上述九九乘法表的解釋，這邊先直接破題。要印出三角形（或任何形狀），其實就是找出每行遞增時，列的內容變化的規律。我們可以知道，行數是靠外層迴圈控制，列的內容則是靠內層迴圈控制。也就是說，外層迴圈執行時做出「行」，此時內層迴圈完全執行，噴出空格或星星產生「列」。基本款與進階款的規則分析如下圖：

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://miro.medium.com/max/875/0*gxMMhRei5QdlmwwT">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: #999;
    padding: 2px; font-size:14px">題解圖示／來源：Yvonne Shih</div>
</center>

基本款的三角形，「每一行」要做的其實就是產生「星星」加「換行」，它的規律是「行數」遞增時，「星星」也遞增。使用程式碼來解釋，外層迴圈控制行數遞增與換行，內層迴圈控制遞增的星星 ：

```javascript
// 印出 5階 直角三角形
let str1 = "";
for (let i = 1; i <= 5; i++) {
  //外部：控制行數 + 換行
  for (let j = 0; j < i; j++) {
    //內部：控制*數
    str1 += "*";
  }
  str1 += "\n";
}
console.log(str1);

// 印出 5階 直角角形-repeat解：
// str.repeat(count) 指示重複字串次數
let star1 = "*";
for (let i = 1; i <= 5; i++) {
  console.log("*".repeat(i));
}
```

【進階款】

進階款的規則比較複雜一點，但也可以拆解成「每一行」要產生「空格」+「星星」+「換行」。它的規律是「行數」遞增時，每次產生「5 -  行數」的「空格」，以及「2 \* 行數 -1 」的「星星」。使用程式碼來解釋，外層迴圈控制行數遞增與換行，兩個內層迴圈控制有規律變化的空格數與星星數：

```javascript
// 印出 5階 等腰三角形
let str2 = "";
for (let i = 0; i <= 5; i++) {
  let space = "";
  for (let h = 1; h <= 5 - i; h++) {
    //列印空格
    space += " ";
  }
  let star = "";
  for (let m = 1; m <= 2 * i - 1; m++) {
    //列印*號
    star += "*";
  }
  str2 += space + star + "\n";
}
console.log(str2);

// 印出 5階 等腰三角形-reapeat解
let star2 = "*";
let space = " ";
for (let i = 1; i <= 5; i++) {
  console.log(space.repeat(5 - i) + star2.repeat(2 * i - 1));
}
```

參考資料：

- [JavaScript 中的巢狀迴圈（Delft Stack）](https://www.delftstack.com/zh-tw/howto/javascript/nested-for-loops-javascript/)
- [使用 JavaScript 中的 for 迴圈列印三角形（程式人生）](https://www.796t.com/article.php?id=216199)
