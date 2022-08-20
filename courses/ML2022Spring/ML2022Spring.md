# MACHINE LEARNING 2022 SPRING

## 機器學習基本概念簡介

### 何謂機器學習

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
:::info 
$\eta$ : learning rate *(a hyperparameters 自己設定的參數)*<br>
larning rate越高，學習越快 *(數值變化快)*
:::

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

### 模型
Linear Models很顯然是不夠的，它有著很大的限制，稱作**Model Bias**，使得這個模型無法模擬真實的狀況，因此我們需要更複雜、更有彈性的模型
![](https://i.imgur.com/VMYl3Pd.png)

**Piecewise Linear Curves**
上面這條紅線其實就是Piecewise Linear Curves *(分段線性曲線)*，我們可以發現這種曲線可以整理成下列的式子:<br>
<font color = "red">red curve</font> = constant + <font color = blue>sum of a set of blue cruve</font>

![](https://i.imgur.com/C3CjdKW.png)

如圖，利用不同的<font color = blue>藍色曲線</font>加上一個常數，就形成了<font color = red>紅色分段線性曲線</font> *(<font color = red>red curve</font> = 0 + 1 + 2 + 3 curves)*

**Q: 但x和y的關係不一定是Piecewise Linear Curves啊，那該怎麼辦?**<br>
*A: 先在曲線上取幾個點，再連起來形成Piecewise Linear Curves，只要點取得夠好或**夠多**，就能和原本的曲線非常接近*
![](https://i.imgur.com/rdsLvdH.png)

**Q: 那要如何得到<font color = blue>藍色曲線</font>呢?**<br>
*A: 我們可以用**Sigmoid Function**來嘗試逼近它*
![](https://i.imgur.com/HJf0UyJ.png)
而我們上面一直在講的<font color = blue>藍色曲線</font>則是叫做**Hard Sigmoid**
![](https://i.imgur.com/r73qiKr.png)
透過改變<font color = blue>$w$</font>、<font color = green>$b$</font>和<font color = red>$c$</font>，就能去逼近出各種不同的Sigmoid Function
![](https://i.imgur.com/x87SuGf.png)
![](https://i.imgur.com/Y8ccLGp.png)
![](https://i.imgur.com/t1BuUC8.png)

回到這一張圖...
![](https://i.imgur.com/VfIYzfK.png)
我們就成功把<font color = red>紅色分段曲線</font>表示出來了<br>

**整理一下我們的新模型:**
* 單個feature
$$y = b + wx_1\\\downarrow\\ y = b + \sum_i \color{red}{c_i}sigmoid(\color{green}{b_i} + \color{blue}{w_i}x_1)$$
* 多個feature
$$y = b + \sum_j w_jx_j\\\downarrow\\ y = b + \sum_i \color{red}{c_i}sigmoid(\color{green}{b_i} + \sum_j \color{blue}{w_{ij}}x_j)$$
下面實際演示了這個模型...
![](https://i.imgur.com/UQnR9LT.png)
可以將 $r_1$、 $r_2$ 和 $r_3$ 的運算簡寫成如下:
![](https://i.imgur.com/CvtALd5.png)<br>
接著，將 $r_i$ 代入 $sigmoid$ 函數得到 $a_i$ (可表示成 $a = \sigma(r)$， $a$ 、 $r$ 是矩陣， $\sigma$ 是 $sigmoid$ )，最後加上 $b$ 得到 $y$ ，如下圖:
![](https://i.imgur.com/RH9MMiw.png)
經過整理後得到 $y$ 的線性代數表達法:
![](https://i.imgur.com/w42Xv6C.png)
接著將每個未知參數組成一個很長的向量矩陣 $\theta$:
![](https://i.imgur.com/Dd67qNp.png)
這樣我們就重新定義了機器學習的第一步
![](https://i.imgur.com/DkoyRrH.png)

### 新模型的Loss函數 & 最佳化

#### Loss函數

代入的參數由原本的 $w$ 和 $b$ 換成了 $\theta$
$$L(w, b)\rightarrow L(\theta)$$
![](https://i.imgur.com/CBf2PmY.png)
可以發現，除了參數不一樣以外，其他都和上面介紹的Loss函數一樣

#### 最佳化

$$
\theta^* = \arg\min\limits_{\theta}L, 
\theta = 
\left[
\begin{matrix}
& \theta_1\\
& \theta_2\\
& \theta_3\\
& \vdots &
\end{matrix}
\right]
$$
* (Randomly) Pick initial values $\theta^0$
* Compute gradient $g$
$$
g = 
\left[
\begin{matrix}
& \frac{\delta L}{\delta\theta_1}|_{\theta = \theta^0} \\
& \frac{\delta L}{\delta\theta_2}|_{\theta = \theta^0} \\
& \vdots &
\end{matrix}
\right],
\left[
\begin{matrix}
& \theta_1^1 \\
& \theta_1^2 \\
& \vdots &
\end{matrix}
\right] \leftarrow
\left[
\begin{matrix}
& \theta_0^1 \\
& \theta_0^2 \\
& \vdots &
\end{matrix}
\right] -
\left[
\begin{matrix}
& \color{red}{\eta}\frac{\delta L}{\delta\theta_1}|_{\theta = \theta^0} \\
& \color{red}{\eta}\frac{\delta L}{\delta\theta_2}|_{\theta = \theta^0} \\
& \vdots &
\end{matrix}
\right]
$$
$$g = \nabla L(\theta^0), \theta^1 = \theta^0 - \color{red}{\eta}g$$
* 不停地做直到gradient為零向量 *(Zero Vector)* 或不想做為止

**實作上，我們會將N筆資料切成一個一個Batch...**
![](https://i.imgur.com/lomaHC8.png)
每做一個Batch就會**update**一次，把所有Batch都做完一輪稱為**1 epoch**

:::success
#### Example 1
* 10000 examples (N = 10000)
* Batch size is 10 (B = 10)<br>
How many update in **1 epoch**?<br>
*A: 10000 / 10 = **1000 updates***
#### Example 2
* 1000 examples (N = 1000)
* Batch size is 100 (B = 100)<br>
How many update in **1 epoch**?<br>
*A: 1000 / 100 = **10 updates***
:::

### ReLU

其實不一定要像上面透過**Sigmoid**函數來近似，也可以將<b><font color = blue>Hard Sigmoid</font></b>看成兩個**Rectified Linear Unit (ReLU)**的加總
![](https://i.imgur.com/2a4G8kX.png)
![](https://i.imgur.com/yAt23h2.png)
:::info
這裡的 $\color{red}{2}i$ 是因為要合成1條**Hard Sigmoid**需要2條**ReLU**
:::