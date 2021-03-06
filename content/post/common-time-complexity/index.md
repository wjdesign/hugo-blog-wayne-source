+++
author = "Wayne"
title = "初學者學演算法｜從時間複雜度認識常見演算法"
date = "2022-07-23"
description = "最常見的六種演算法案例，分別分析六種不一樣的時間複雜度，並使用 Python 程式語言進行程式碼的實作。"

tags = [
    "演算法",
    "時間複雜度"
]

categories = [
    "演算法"
]

series = ["Themes Guide"]
aliases = ["common-time-complexity"]
image = "https://i.imgur.com/TqtfUrE.jpg"
+++

<style>
.focus {
    background: #f1e2e2;
    color: #d62c2c;
    padding: 0 5px;
}
</style>

[參考網站 - 初學者學演算法｜從時間複雜度認識常見演算法](https://medium.com/appworks-school/%E5%88%9D%E5%AD%B8%E8%80%85%E5%AD%B8%E6%BC%94%E7%AE%97%E6%B3%95-%E5%BE%9E%E6%99%82%E9%96%93%E8%A4%87%E9%9B%9C%E5%BA%A6%E8%AA%8D%E8%AD%98%E5%B8%B8%E8%A6%8B%E6%BC%94%E7%AE%97%E6%B3%95-%E4%B8%80-b46fece65ba5)  

---

## 溫故知新  

- 演算法的簡單定義：`輸入 + 演算法 = 輸出`  
- 時間複雜度：衡量演算法執行好壞的工具  
- 大 O 符號：用來描述演算法在輸入 n 個東西時，所需時間與 n 的關係  
- 在 n 非常大時，好的演算法設計可以省下非常多時間  
- 演算法的速度不是以秒計算，而是以步驟次數  
- 實務上，我們只會紀錄最高次方的那一項，並忽略其所有的係數  

---

## 目錄：常見的六種時間複雜度與演算法  

- O(1)：陣列讀取  
- O(n)：簡易搜尋  
- O(log n)：二分搜尋  
- O(nlogn)：合併排序  
- O(n²)：選擇排序  
- O(2^n)：費波那契數列  

---

### O(1)：陣列讀取  

#### 說明

時間複雜度為 O(1) 的演算法，代表著不管你輸入多少個東西，程式都會在同一個時間跑完。在程式設計中，最簡單的例子就是讀取一個陣列中特定索引值的元素(程式麻瓜先別急著吐血，且讓我們在下面慢慢說明)。  

#### 陣列讀取  

陣列是程式中儲存東西的一種容器，我們可以想像成一排已經編號好的櫃子。每一個櫃子上的編號我們稱為「索引值」（Index，在程式中這個編號通常從 0 開始），而櫃子裡的物品我們稱為「元素」。例如：假設神奇寶貝大師小明在一個名叫 Pokemons 的陣列裡依序放入他的神奇寶貝們，我們來複習一下陣列、元素、索引值的關係：  

![](https://i.imgur.com/KhnnEpC.jpg)  

在程式碼中我們把七隻神奇寶貝這樣表達：  

```Python
Pokemons = ["卡丘","胖丁","尼龜","比獸","呆獸","種子","小剛"]
```

這時，假設我們想要知道在這個 Pokemons 陣列中任一個編號所對應到的神奇寶貝，我們都只需要把這個編號對應的元素印出來，就能知道對應的神奇寶貝是誰了。如果我想知道這個陣列中的第 n 號櫃的神奇寶貝是誰（以下假設我們想知道 n= 0），在程式碼中我們可以這樣表達：  

```Python
n = 0
print(Pokemons[n])

>> "卡丘"
```

陣列讀取時，因為我們已經知道櫃子的索引值，不管放入的 n 等於多少，程式都可以在 “一個步驟” 就到達 n 所對應到編號的櫃子並取出該元素，像這樣的案例，我們就會說陣列讀取演算法的時間複雜度為 O(1)。  

---

### O(n)：簡易搜尋  

#### 說明  

時間複雜度為 O(n) 的演算法，代表著執行步驟會跟著輸入 n 等比例的增加。例如當 n = 8，程式就會在 8 個步驟完成。最簡單的例子，就是所謂的簡易搜尋。  

> 這邊要特別提醒一點，通常程式步驟的時間複雜度會是用程式執行會碰到的最壞狀況 (Worst Case) 來表示，詳細例子我們可以在下面看到。  

#### 簡易搜尋  

讓我們沿用上一段的 Pokemons 陣列作為例子。Pokemons 這一排櫃子裡有八隻神奇寶貝，假設每個櫃子的門都被關上，我們事前也不知道各個神奇寶貝的位置，這時如果想要知道「呆獸」神奇寶貝在哪裡時，我們第一個想到的方法會是什麼呢？  

最直觀地想，我們會從第一個櫃子開始試，一次開一個櫃子，直到找到「呆獸」為止。像這樣的搜尋方法，就是最經典簡單的「簡易搜尋」。  

在程式碼中，簡易搜尋的方法可以這樣表達：  

```Python
Pokemons = ["卡丘","胖丁","尼龜","比獸","呆獸","種子","小剛"]
for Pokemon in Pokemons:
  if Pokemon == "呆獸":
    print("找到呆獸！")
    break
  else:
    print("這個櫃子裡不是呆獸")
```

觀察上面的程式碼時，我們可以發現，如果呆獸在第 0 號櫃，我們一個步驟就會找到它，但如果他是在第 6 號櫃，我們要花七個步驟才能找到他。  

還記得我們在上面提過的小小提醒嗎？我們通常會用程式執行會碰到的「最壞狀況」來決定複雜度的表示，也因此，當我們要從 n 個櫃子中找到一隻特定的神奇寶貝，我們最慘最慘的情況需要花剛好 n 個步驟才能找到（想像要找的神奇寶貝在最後一個櫃子的情況）。像這樣的案例，我們就會說簡易搜尋演算法的時間複雜度為 O(n)。  

---

### O(log n)：二分搜尋法  

#### 說明  

時間複雜度為 O(log n) 的演算法（這邊的 log 都是以二為底），代表當輸入的數量是 n 時，執行的步驟數會是 log n。（讓忘記 log 是什麼的同學們複習一下，當 log n = x 的意思是 n = 2^x，如果這部分的腦細胞尚未復活，且讓我們先記住 n = 2^x，再來看看例子）。  

舉例來說，當 n = 4，程式會在 2 個步驟完成（4 = 2²）；n = 16 時，程式會在 4 個步驟完成（16 = 2⁴），以此類推。  

在程式中，O(log n) 的最常見例子是二分搜尋法。  

#### 二分搜尋法  

假設我們在一本字典中想要找到一個單字，這個字以 W 開頭，我們可以用前面提過「簡易搜尋」的邏輯，從第一頁的 A 開始找起，一個一個找到天荒地老海枯石爛。也可以用更珍惜生命的方式，直接翻到字典的後面，找到以 W 開頭的第一個字後再開始往後找。  

同樣的邏輯，假設有一長串有小到大排序好的數字們，我要在其中找特定一個數字，我們一樣可以從第一個往後一個一個檢查。但假設我們想要更珍惜生命，聰明的讀者可能已經想到了我們在「終極密碼」這種遊戲中會使用的策略，也就是每次都先檢查最中間的數字，如果中間的數字比我們要找的數字大，我們要找的數量就只剩原本的一半（因為在後段的數字顯然都會比我們要找的數字大），這樣的方法，就稱作二分搜尋法。  

舉一個實際的例子，假設今天有一排編號好的櫃子，裡面擺著八個由小到大排序好的數字。假設我們知道裡面的數字包含 55，但我們不知道在哪一個編號櫃子中。讓我們來比較簡易搜尋（從第一格往後一個一個檢查）跟二分搜尋法有什麼差別。  

![](https://i.imgur.com/a6K8ZNC.jpg)  

從上面的圖可以看到，一般的搜尋方法需要花五個步驟才能找到 55。  

![](https://i.imgur.com/N24DLqt.jpg)  

而在二分搜尋法中，我們先打開最中間的櫃子，發現裡面的數字是 41。因為 55 比 41 大，因此我們知道從一號櫃到三號櫃都不會有 55，接下來只需要檢查五號櫃到七號櫃。  

同樣的邏輯，我們打開剩下三個可能性中最中間的櫃子，發現六號櫃裡面的數字是 61，因為 61 比 55 大，我們可以知道七號櫃的數字一定也比 55 大，得知 55 一定就在五號櫃之中。  

接下來，要再次來關心兩個搜尋方法的時間複雜度。簡易搜尋的情況中，我們可以輕鬆地知道最壞的情況就是剛好七個步驟（要找的數字是 80 ）。而二分搜尋法，我們可以先練習去計算各種情況需要的步驟，而最終的答案如下表：  

![](https://i.imgur.com/bf1ZxXK.jpg)  

從上表我們可以發現，二分搜尋法最慘最慘，也只需要三個步驟。  

推廣到有 n 個櫃子時，我們可以發現：二分搜尋法在每進行一個步驟時，就可以排除掉一半的可能性。每次都能減少一半，因此二分搜尋法最糟最糟也只需要以 2 為底的 log n 個步驟就能完成。  

二分搜尋法在程式碼中的例子，對於程式新手可能需要花比較多的理解。如果你是對程式有一定理解的人，可以嘗試動手實做看看。而如果下方的程式碼對於讀者還有些吃力的話，也可以先多多熟悉語法後回來複習即可。  

```Python
Numbers = [5,17,33,41,55,61,80]
Find = 55
​
low = 0
high = len(Numbers) - 1
​
while low <= high:
    mid = (low + high) // 2
    if Numbers[mid] > Find:
        high = mid - 1
    elif Numbers[mid] < Find:
        low = mid + 1
    else:
        break
​
print(mid)
```

---

### 小結  

在這篇文章中，我們分別了解了 O(1)、O(n)、O(logn) 的時間複雜度，以及對應到的三個常見演算法。而在接下來的文章中，我們會開始認識新朋友，在演算法中佔有重要地位的「排序法」，以及在更進階的例子。  

---

