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
