+++
author = "Wayne"
title = "彭彭的課程 - 氣泡排序、插入排序的實作與分析"
date = "2022-07-21"
description = "JavaScript 資料結構與演算法：氣泡排序 Bubble Sort、插入排序 Insertion Sort 實作與分析。"

tags = [
    "演算法",
    "排序"
]

categories = [
    "演算法"
]

series = ["Themes Guide"]
aliases = ["bubble-sort-and-insertion-sort"]
image = "hqdefault.jpg"
+++

<style>
.focus {
    background: #f1e2e2;
    color: #d62c2c;
    padding: 0 5px;
}
</style>

[參考網站 - JavaScript 資料結構與演算法：氣泡排序 Bubble Sort、插入排序 Insertion Sort 實作與分析 - 彭彭直播](https://youtu.be/i-0wxW5Aun4)  

---

## 排序演算法  
### 氣泡排序法(bubble sort)  

![](https://i.imgur.com/dhpsLsT.png)  

#### 概要  

- 使用雙層迴圈，**由後往前**。  
- 每輪**固定最右邊**的值，接著倆倆比較大小，將大的**放右邊**。  
- 下輪則 **- 1**。  
- 完畢後即可排序完畢。  
- 執行的總輪數為**陣列長度 - 1**。  

#### 時間複雜度  

![](https://i.imgur.com/zI5aeQp.png)  

- 如果陣列長度是 **4**，要比對 **3+2+1** 總共 **6** 次。  
- 如果陣列長度是 **7**，要比對 **6+5+...+1** 總共 **21** 次。  
- 如果陣列長度是 **n**，要比對 **(n-1)+(n-2)+...+1** 總共  
**(n * (n - 1)) / 2** = <mark>n²/2 - n/2</mark> 次  
- 搜尋所需時間隨著陣列的長度  
呈<mark>平方成長 O(N²)</mark>。  

#### 假設  

![](https://i.imgur.com/QIeIkQk.png)  

- 可以加入一個 **flag** 來做判定，假設比較完第一輪發現沒有交換的情況發生，則代表已經排序完成，不需要再跑下一輪，即可稍微優化排序。  

#### 實作  

```javascript
// 實作氣泡排序演算法
function bubbleSort(arr){ // arr 是一個數字陣列
    for(let i=arr.length-1;i>=1;i--){
        let swap=false; // 假設沒有交換發生
        for(let j=0;j<i;j++){
            if(arr[j]>arr[j+1]){ // 如果順序不對，交換
                let temp=arr[j];
                arr[j]=arr[j+1];
                arr[j+1]=temp;
                swap=true; // 紀錄發生交換
            }
        }
        if(!swap){ // 發現一整輪中都沒有交換發生，直接判定排序完成
            break;
        }
    }
}

let data = [1, 6, 3, 4];
console.log(bubbleSort(data));
```

###### 輸出：  
```console
> [1, 3, 4, 6]
```

---

### 插入排序法(insertion sort)  

![](https://i.imgur.com/qnJOrAZ.png)  

#### 概要  

- 使用雙層迴圈，**由前往後**。  
- 從第二筆開始，每輪將該筆資料往前比較大小，將大的**放右邊**：每輪比較從 **(i - 1) ~ 0**。  
- 下輪則 **+ 1**。  
- 完畢後即可排序完畢。  
- 執行的總輪數為**陣列長度 - 1**。  

#### 時間複雜度(複雜度同氣泡排序法)  

![](https://i.imgur.com/FyUSUGl.png)  

- 如果陣列長度是 **4**，要比對 **1+2+3** 總共 **6** 次。  
- 如果陣列長度是 **7**，要比對 **1+2+...+6** 總共 **21** 次。  
- 如果陣列長度是 **n**，要比對 **1+2+...+(n-1)** 總共  
**(n * (n - 1)) / 2** = <mark>n²/2 - n/2</mark> 次  
- 搜尋所需時間隨著陣列的長度  
呈<mark>平方成長 O(N²)</mark>。  

#### 假設  

![](https://i.imgur.com/2u7G5aD.png)  

- 假設當前比對的值與第一個要比較的值一比較，恰好正確，則代表前面皆已經排序完成，可以進到下一輪。  

#### 實作  

```javascript
// 實作插入排序演算法
function insertionSort(arr){ // arr 是一個數字陣列
    for(let i=1;i<arr.length;i++){
        for(let j=i-1;j>=0;j--){
            if(arr[j]>arr[j+1]){ // 如果順序不對，交換
                [arr[j], arr[j+1]]=[arr[j+1], arr[j]]; // javascript 交換的語法糖
            }else{ // 任何一次比較，發現順序對了，這一輪就不用繼續了
                break;
            }
        }
    }
}

let data = [1, 6, 3, 4];
console.log(insertionSort(data));
```

###### 輸出：  
```console
> [1, 3, 4, 6]
```

---

### 大型資料量的進階探討  

O(N²)：(讀作 big-O N平方) 是相當可怕的，排序的執行時間將會是資料量的平方倍數成長。  

###### 演示：  

```javascript
// 產生隨機資料
let data=[];
for(let i=0;i<100000;i++){
    data.push(Math.random()*1000000);
}
// 資料量是 100,000，我的演算法時間複雜度是 O(N^2)，預期要花 100,000^2 = 10,000,000,000 次的比較運算
// 我們的電腦一秒鐘跑 10 億個指令(粗略預估 1 GB)
console.time();

// 插入排序法，大約跑了10幾秒
//insertionSort(data);

// 使用 JavaScript 內建的排序功能 sort()，大約跑了 0.2 ~ 0.3 秒
// 很有機會是使用快速排序 Quick Sort(快速排序法) 或其變形
data.sort();

console.timeEnd();
```

---