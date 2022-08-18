# Lecture 1

## 機器學習基本概念簡介

**Q: 什麼是機器學習?**<br>
*A: 概括來說，讓機器具備找一個函式的能力!*
![](https://i.imgur.com/xNtqsVZ.png)

### 函數的類型

**上面提到機器要找一個函數，這個函數被大致分為幾種**
* **Regression:** 輸出為一個數值<br>
![](https://i.imgur.com/cxaTsKh.png)
* **Classification:** 從既有的選項 *(classes/類別)* 中，選擇其一輸出
![](https://i.imgur.com/CKs1af4.png)
![](https://i.imgur.com/vXEwi7c.png)
* **Structured Learning:** 讓機器產生一個有結構的物件 *(e.g. 畫一張圖、寫一篇文章...)* ，讓機器學會創造

### 如何找一個函數

**以簡單預測YouTube流量為例...**
1. **Find with Unknown Parameters** 寫出帶有未知參數的函式
![](https://i.imgur.com/jUCKe8g.png)
2. **Define Loss form Training Data** 定義損失函數
    * Loss is a function of parameters: L(b, w)
    * Loss: how good a set of value is

![](https://i.imgur.com/LyQPab9.png)

Loss: $$L = \frac{1}{N}\Sigma e_n$$

將每一天的誤差值加總起來取平均，就能得到損失函數的值<br>
計算誤差值的方式大致上有下列2種方法:
* **MAE** Mean Absolute Error
    $e = |y - \hat{y}|$
* **MSE** Mean Square Error
    $e = (y - \hat{y})^2$

依照需求和對任務的理解作選擇<br>
*如果* $y$ *和* $\hat{y}$ *都是機率分佈的話，則會選擇Cross-entropy(之後再說)*

**Error Surface:**
將各種參數組合出來的Loss值繪製成等高線圖
![](https://i.imgur.com/OWUXEpy.png)

3. **Optimization** 最佳化 
$$w^*, b^* = \arg\min\limits_{w, b}L$$
*找一個最好的* $w$*和* $b$ *(* $w^*$ & $b^*$ *)，使得Loss的值最小*
**Gradient Descent:**
先遮住另一個參數，我們先看單一的參數...
![](https://i.imgur.com/HKcwjqt.png)
> $\eta$ : learning rate *(a hyperparameters 自己設定的參數)*
> larning rate越高，學習越快 *(數值變化快)*

如圖，Gradient Descent顯而易見的問題即是local minima問題，通常無法找到golbal minima，但老師提到local minima其實是個假議題，做Gradient Descent時會遇到的真正難題並不是local minima問題 *(之後再提Gradient Descent真正的痛點)*<br>
單一的參數理解之後，多個參數也是相同概念...
* (Randomly) Pick initial values $w^0, w^b$
* Compute 
$$w^1\leftarrow w^0-\eta\frac{\delta L}{\delta w}|_{w = w^0, b = b^0}$$
$$b^1\leftarrow b^0-\eta\frac{\delta L}{\delta b}|_{w = w^0, b = b^0}$$
* Update $w$ and $b$ interatively

### 訓練 & 預測
上面的步驟其實就是在訓練
![](https://i.imgur.com/Nt67jPa.png)
如圖，在已知的資料上的最小Loss是0.48k，而在預測的數據上，Loss則是來到0.58k
![](https://i.imgur.com/4D6qlbk.png)
![](https://i.imgur.com/w1NUdrS.png)<br>
根據第一次預測的結果，我們可以發現這些資料有存在週期性 *(在此為7天一循環，星期四和星期五的觀看人數都會減少)*，利用這個週期性來嘗試修改模型...<br>
以上這些`feature * weight + bias`的模型就稱為 **Linear Models**