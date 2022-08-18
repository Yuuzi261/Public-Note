# Lecture 1

## 機器學習基本概念簡介

**Q: 什麼是機器學習?**<br>
*A: 概括來說，讓機器具備找一個函式的能力!*
![](https://github.com/GoldOrange261/Public-Note/blob/main/courses/ML2022Spring/screenshots/1.png)

### 函數的類型

**上面提到機器要找一個函數，這個函數被大致分為幾種**
* **Regression:** 輸出為一個數值<br>
![](https://github.com/GoldOrange261/Public-Note/blob/main/courses/ML2022Spring/screenshots/2.png)
* **Classification:** 從既有的選項 *(classes/類別)* 中，選擇其一輸出
![](https://github.com/GoldOrange261/Public-Note/blob/main/courses/ML2022Spring/screenshots/3.png)
![](https://github.com/GoldOrange261/Public-Note/blob/main/courses/ML2022Spring/screenshots/4.png)
* **Structured Learning:** 讓機器產生一個有結構的物件 *(e.g. 畫一張圖、寫一篇文章...)* ，讓機器學會創造

### 如何找一個函數

**以簡單預測YouTube流量為例...**
1. **Find with Unknown Parameters** 寫出帶有未知參數的函式
![](https://github.com/GoldOrange261/Public-Note/blob/main/courses/ML2022Spring/screenshots/5.png)
2. **Define Loss form Training Data** 定義損失函數
    * Loss is a function of parameters: L(b, w)
    * Loss: how good a set of value is

![](https://github.com/GoldOrange261/Public-Note/blob/main/courses/ML2022Spring/screenshots/6.png)

> Loss: $L = \frac{1}{N}\Sigma e_n$

將每一天的誤差值加總起來取平均，就能得到損失函數的值<br>
計算誤差值的方式大致上有下列2種方法:
1. **MAE** Mean Absolute Error
    $e = |y - \hat{y}|$
2. **MSE** Mean Square Error
    $e = (y - \hat{y})^2$

依照需求和對任務的理解作選擇<br>
*如果* $y$ *和* $\hat{y}$ *都是機率分佈的話，則會選擇Cross-entropy(之後再說)*

**Error Surface:**
將各種參數組合出來的Loss值繪製成等高線圖
![](https://github.com/GoldOrange261/Public-Note/blob/main/courses/ML2022Spring/screenshots/7.png)