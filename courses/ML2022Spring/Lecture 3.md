# Lecture 3 卷積神經網路 (Convolutional Neural Networks, CNN)

## Image Classification

我們先假設每張輸入進模型的圖片大小是固定的，實際上現今普遍的影像辨識系統在丟入圖片之前也是會先 Rescale 成大小一樣。我們模型的目標是分類，所以將類別表示成 One-Hot Vector 的形式，即為下圖的 $\hat{y}$，假設目標是貓，貓所對應的 dimension 就會是1，而其他 dimension 為0，這個 dimension 的長度決定了一個模型可以辨識的種類多寡

![](https://i.imgur.com/UKjO0oY.png)

模型的輸出經過 $softmax$ 之後表示為 $y'$，我們會希望 $y'$ 和 $\hat{y}$ 的 cross entropy 越小越好。接下來會遇到的問題是，怎麼把一張圖片當成模型的輸入呢？我們必須先了解對電腦來說，一張影像是什麼樣的東西

![](https://i.imgur.com/hkmYscn.png)

其實對電腦來說，圖片就是一個3維的 Tensor（超過2維的矩陣我們就會稱呼他為 Tensor），一維代表圖片的長，一維代表圖片的寬，還有一維代表圖片的 channel 數目，一張彩色的圖片是由 RGB 三色所組成，所以上圖3個 channel 就是代表 RGB 這三個顏色。接下來將這個三維的 Tensor 拉直成一個巨大的向量，就可以放進 Network 去做訓練了

![](https://i.imgur.com/KBgrB5f.png)

接下來我們把這個 vector 輸入到 Fully Connected Network 裡，假設第一層有1000個 Neuron，那麼第一層就會有 $100\times100\times3\times1000=3\times10^7$ 個 weight，如此多的參數雖然可以讓模型更有彈性，但也增加了 overfitting 的風險，所以考慮到影像辨識的特性，其實並不需要全連接，接下來會講述我們對影像辨識這個問題的一些觀察

## Observation 1

對於一個類神經網路裡面的神經元而言，它要做的事就是去偵測圖片中有沒有出現一些特別重要的 pattern，而這些 pattern 是代表某種物件的。如下圖，有一個 Neuron 說它看到了鳥喙、另一個說它看到眼睛、又有一個說它看到鳥爪，綜合起來可能就能辨識出這是一隻鳥。

![](https://i.imgur.com/vXKUwY0.png)

:::success
🤔或許你可能會覺得這個方法不是很聰明，但你有沒有想過人類也是同樣的方法判斷一個物件的呢？

![](https://i.imgur.com/RD5Q5k9.png)

所以即便是人，在判斷一個物件的時候往往還是抓最重要的幾個特徵來辨識，因此對機器來說，上面提到的方法可能是有效的
:::

照上面說的方法，如果一個 Neuron 的工作就是去判斷一張圖片有沒有出現某些重要的 pattern ，那理論上每一個 Neuron 就不需要把一整張圖片當作輸入，這就是第一個觀察，根據這個觀察我們可以做第一個簡化

![](https://i.imgur.com/r120Qrl.png)

我們可以定義某個 Neuron 有一個固定的守備範圍，也就是 **Receptive Field**，對這個 Neuron 來說它只需要關心這個小範圍就可以了，不需要關心整張圖片有什麼東西。至於這個守備範圍要怎麼設計基本上就是自己決定，可以同一個 Receptive Field 有多個 Neuron 守備，Receptive Field 也可以互相重疊，甚至形狀也不一定要是固定大小或是形狀...全都端看自己的需求

![](https://i.imgur.com/uxNKqC2.png)

雖然上面說 Receptive Field 可以任意設計，但還是要介紹一下平常最常見最經典的安排方式：

![](https://i.imgur.com/LcVitq7.png)

其中 kernel size 和 stride 是 Hyperparameter，kernel size 和 stride 通常不會設太大，7x7和9x9的 kernel size 其實就算很大了，至於 stride 不會設很大的理由是我們希望每個 Receptive Field 之間可以有重疊，以免漏掉一些在邊界上的 pattern。如果有 Receptive Field 超出邊界，通常會進行 padding，全部填0、補平均數、或是直接拿邊界補上來都是可行的方法，並且一個 Receptive Field 通常會有一組 Neuron 守備，不會只有一個

## Observation 2

講完第一個觀察，你可能就會有這個疑問，一個 pattern 可能會出現在圖片的各個區域中，這樣會構成什麼問題嗎？答案其實是不會，因為每個 Receptive Field 都有 Neuron 在守備，所以不管出現在哪裡都是可以被偵測出來的

![](https://i.imgur.com/i0HDgju.png)

:::warning
❓但這時就會出現另一個問題，既然每個 Receptive Field 都有一個 Neuron 守備某個 pattern，那我們有需要每一個 Receptive Field都去放一個獨立偵測鳥嘴的 Neuron 嗎？
:::

這樣做顯得很沒效率而且浪費資源，我們實際上不需要這麼多的 Neuron 和 weight 去看每一個 Receptive Field 有沒有特定的 pattern ，既然做的事情是一樣的，那麼我們是不是能夠讓某些 Neuron 共享 weight 呢？

![](https://i.imgur.com/nylB5RL.png)

沒錯於是我們就有了第二個簡化！不同的 Receptive Field 可能會有不同的 Neuron 共享同樣的參數，雖然參數一樣，但因為輸入不同所以輸出也不會一樣，所以理所當然的我們不會讓相同 Receptive Field 裡的 Neuron 共享參數，不然這樣輸出就會變得一樣了。至於如何共享也是可以自由決定的，不過和第一個簡化一樣會介紹常見的共享方法是如何設定的

![](https://i.imgur.com/1d4Kl0O.png)

其實設定的方法非常直覺，就是每組 Receptive Field 的參數都是一樣的，每個守備範圍都有一個相同參數的 Neuron 來偵測某個特定的 pattern，我們稱呼這些 Neuron 為 **Filter**

## Benefit of Convolutional Layer

給 Fully Connected Layer 加上上述兩種簡化之後，使得彈性下降、bias 變大，但這並不代表不好，而是更能適應圖像辨識這項任務，儘管 Fully Connected Layer 可以用於很多任務，卻沒有一項是精通的，而且容易 overfitting。這個加上兩項限制的 Layer，就被稱作 **Convolutional Layer**，至於用到 Convolutional Layer 的 Network 就是我們熟知的 **CNN**

![](https://i.imgur.com/8k9eSNd.png)

:::info
接下來我們會以另一種角度介紹 CNN，這個版本的故事也會解釋為何之前提到說3x3的 Receptive Field 就足夠了
:::

![](https://i.imgur.com/SCoAxsL.png)

所謂 **Convolution** 就是指裡面有很多 **Filter** 的這種結構，這些 **Filter** 的大小是 $3\times3\times channel$ 的大小（跟之前說的一樣其實不一定是 $3\times3$，但一般都是這個大小），如果是彩色圖片，channel 為 3、如果是黑白圖片，channel 為 1。每個 **Filter** 的目的就是去抓取圖片中的特定 pattern，那麼 **Filter** 是如何去抓取 pattern 的呢？

![](https://i.imgur.com/T62Qu0B.png)

**Filter** 的數值理論上是未知的，必須透過 gradient descent 來找出來，這裡假設已經訓練完了，並且這是一張黑白的圖所以只有一個 channel

![](https://i.imgur.com/hrdRJSA.png)

這裡舉的例子 stride = 1，也就是 **Filter** 每次移動一個像素，每次都跟圖片的數值做相乘，於是得到了右下角的圖。我們可以發現，當圖片出現這種從左上到右下的 pattern 時，相乘出來的結果會最大，這兩個出現 3 的地方，也對應了圖片中左上跟左下出現的相同 pattern

![](https://i.imgur.com/HL76u9t.png)

接下來每個 **Filter** 都做相同的事，所以有幾個 **Filter** 就會得到相同數量的圖，這些圖我們稱作 **Feature Map**

![](https://i.imgur.com/LxW8WB9.png)

如同上面所說，如果有 64 個 **Filter**，那麼最後得到的 Feature Map 就會有 64 個 channel，我們可以將這個 Feature Map 視為一張新的圖片。接下來，我們當然可以疊很多層的 Convolution，這個第二層的 Convolution 的 Filter 就會變成 $3\times3\times64$，這裡的 64 就是上一層 Convolution 的 Filter 數目

![](https://i.imgur.com/Mp0mJ2Q.png)

:::warning
❓回到之前留下的一個問題，如果我們的 Filter 一直設 $3\times3$ 會不會無法偵測到比較大的 pattern 呢？
:::

從下面這張圖就可以很明顯地知道不會！

![](https://i.imgur.com/7yyakAo.png)

儘管我們的 Filter 一直都設 $3\times3$ 但對第二層的 Filter 來說，一個數字就代表著原圖的 $3\times3$，所以第二層的一個 Filter 其實可以守備到原圖的 $5\times5$ 大小，因此只要 Network 疊得夠深，就不用怕偵測不到比較巨大的 pattern！

## Observation 3

進行影像辨識的時候，其實還有第三個常用的東西稱作 **Pooling**，目的是為了減少運算量，但出來的結果也會稍微差一點，其原理非常簡單，就是縮小圖片的大小

![](https://i.imgur.com/YD837g2.png)

Pooling 本身沒有參數，也就是說它不是一個 Layer，它沒有 weight 所以也沒有要 learn 的東西，比較像是 Sigmoid 和 ReLU 這種，這裡介紹一個常見的 Pooling，**Max Pooling**。簡單來說就是在一個範圍內選最大的值作為代表，當然這個範圍可以自己決定，也不一定要選最大的值作為代表，比如取平均數或是幾何平均數之類的，但這就是別種的 Pooling 了

![](https://i.imgur.com/wDyYrod.png)

一般在實作上會將 Convolution 和 Pooling 交替使用，但近年來影像辨識的 Network 的設計開始棄用 Pooling 了，畢竟現在運算資源越來越多，如果全 Convolution 可以跑得起來的話就不需要多一層 Pooling 反而更好

![](https://i.imgur.com/O0fQfEr.png)

最後總結一下一個 CNN 大致的流程，一張圖片經過 Convolution 和 Pooling（可有可無）之後，將結果拉直成一個向量，接下來將拉直的向量丟進 Fully Connected Layers 裡面，可能還要經過 softmax，最終獲得影像辨識的結果

![](https://i.imgur.com/b427pYT.png)
